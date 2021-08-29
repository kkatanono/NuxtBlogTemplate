<template>
  <div>
    <header>
      <h1>header</h1>
    </header>
    <nav class="global-nav">
      <ul>
        <li>Menu1</li>
        <li>Menu2</li>
        <li>Menu3</li>
        <li>Menu4</li>
      </ul>
    </nav>

    <div class="wrapper">
      <aside class="left-aside">Left aside</aside>
      <aside class="right-aside">
        Right aside
        <section>
          <h2>プロフィール</h2>
          <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Cumque dolorum officia dignissimos minima! Soluta dolorem ex illo, repellendus hic corrupti nulla deleniti distinctio, nisi unde, corporis magnam cupiditate repellat sapiente?</p>
        </section>
        <section>
          <h2>人気の記事</h2>
          <ul>
            <li>
              今日のごはん
            </li>
            <li>
              今日のごはん

            </li>
            <li>
              今日のごはん

            </li>
            <li>
              今日のごはん

            </li>
            <li>
              今日のごはん

            </li>
          </ul>
        </section>
        
        </aside>
      <main>
        main
        <article>
          <h2>article</h2>
        </article>
      </main>
    </div>

    <footer>
      <p>copyright</p>
    </footer>
  </div>
</template>

<style>
header{
  background-color: rgb(13, 63, 224);
  position: sticky; 
  top: 0;
}
header h1{
  margin: 10px;
  color: #fff;
}

nav {
}
.global-nav ul {
  display: flex;
  list-style: none;
  justify-content: flex-end;
  flex-wrap: wrap;
}
.global-nav li {
  margin: 10px;
}
.wrapper {
  display: flex;
  flex-wrap: wrap;
  margin-top: 30px;
}
@media (max-width: 480px) {
  .wrapper {
    flex-direction: column-reverse;
  }
  .left-aside {
    display: none;
  }
}
aside {
  /* background-color: blue; */
  flex: 1;
  min-width: 300px;
}
.right-aside {
  order: 3;
  background-color: rgb(241, 233, 233);
  min-width: 100%;
}
section{
  margin: 10px;
}
.wrapper main {
  /* background-color: red; */
  flex: 2;
  height: 600px;
}
article {
}
footer {
  text-align: center;
  margin: 10px;
}
</style>

<script>
export default {
  data() {
    return {
      query: '',
      articles: [],
    }
  },
  watch: {
    async query(query) {
      if (!query) {
        this.articles = []
        return
      }

      this.articles = await this.$content('articles')
        .only(['title', 'slug'])
        .sortBy('createdAt', 'asc')
        .limit(12)
        .search(query)
        .fetch()
    },
  },
}
</script>
