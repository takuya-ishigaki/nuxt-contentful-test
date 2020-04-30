<template>
  <div class="container">
    <div>
      <h1 class="title">
        Contentful Test
      </h1>
      <h2 class="subtitle">
        News
      </h2>
      <ul>
        <li v-for="item in news" :key="item.fields.slug">
          {{ item.fields.title }}
        </li>
      </ul>
      <h2 class="subtitle">
        Blog
      </h2>
      <ul>
        <li v-for="item in blog" :key="item.fields.slug">
          <nuxt-link :to="`/blog/${item.fields.slug}/`">{{ item.fields.title }}</nuxt-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import contentfulClient from '~/plugins/contentful.js'

export default {
  async asyncData () {
    const getPosts = async (contentType) => {
      const posts = await contentfulClient.getEntries({
        content_type: contentType,
        limit: 10,
        order: '-sys.createdAt'
      })

      return posts.items
    }

    return {
      news: await getPosts('news'),
      blog: await getPosts('blog')
    }
  },
  mounted () {
    console.log('news', this.news)
    console.log('blog', this.blog)
  }
}
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

ul {
  list-style: none;
}
</style>
