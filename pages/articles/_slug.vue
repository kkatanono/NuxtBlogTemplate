<template>
  <div>
    <!-- <ul>
      <li
        v-for="link of article.toc"
        :key="link.id"
        :class="{ toc2: link.depth === 2, toc3: link.depth === 3 }"
      >
        <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
      </li>
    </ul> -->
    <article>
      <h1>{{ blogs.title }}</h1>
      <p>{{ blogs.date }}</p>
      <nuxt-content :document="blogs" />
    </article>

    <NuxtLink v-if="prev" :to="'/articles/' + prev.slug">
      {{ prev.title }}
    </NuxtLink>

    <NuxtLink v-if="next" :to="'/articles/' + next.slug">
      {{ next.title }}
    </NuxtLink>
  </div>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const blogs = await $content('articles', params.slug || 'index').fetch()

    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    const article = await $content('articles', params.slug).fetch()

    return { blogs, prev, next, article }
  },
}
</script>
