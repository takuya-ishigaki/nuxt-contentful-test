<template>
  <div class="container">
    <h1>{{ blog.fields.title }}</h1>
  </div>
</template>

<script>
import contentfulClient from '~/plugins/contentful.js'

export default {
  async asyncData ({ route }) {
    const getPosts = async (contentType) => {
      const posts = await contentfulClient.getEntries({
        content_type: contentType,
        'fields.slug': route.params.id,
        limit: 10,
        order: 'sys.createdAt'
      })

      return posts.items[0]
    }

    return {
      blog: await getPosts('blog')
    }
  },
  mounted () {
    console.log(this.blog)
  }
}
</script>

<style></style>
