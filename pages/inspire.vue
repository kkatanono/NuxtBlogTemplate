<template>
  <div>
    <h1>aa</h1>
    <input v-model="query" type="search" autocomplete="off" />

    <ul v-if="articles.length">
      <li v-for="article of articles" :key="article.slug">
     <nuxt-link :to="'/articles/'+ article.slug">{{article.title}} {{article.date}}</nuxt-link>
        <!-- <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">{{ article.title }}</NuxtLink> -->
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      query: '',
      articles: []
    }
  },
  watch: {
    async query (query) {
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
    }
  }
}
</script>