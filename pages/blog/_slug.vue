<template>
  <div class="container">
    <h1>{{ blog.fields.title }}</h1>
    <div v-html="toHtmlString(blog.fields.body)"></div>
  </div>
</template>

<script>
import { BLOCKS } from '@contentful/rich-text-types'
import { documentToHtmlString } from '@contentful/rich-text-html-renderer'
import contentfulClient from '~/plugins/contentful.js'

export default {
  async asyncData ({ params }) {
    const getPosts = async (contentType) => {
      const posts = await contentfulClient.getEntries({
        content_type: contentType,
        'fields.slug': params.slug
      })

      return posts.items[0]
    }

    return {
      blog: await getPosts('blog')
    }
  },
  methods: {
    toHtmlString (obj) {
      const options = {
        renderNode: {
          [BLOCKS.EMBEDDED_ASSET]: ({ data: { target: { fields } } }) =>
            `<img src="${fields.file.url}"/>`
        }
      }
      return documentToHtmlString(obj, options)
    }
  }
}
</script>

<style scoped>
/deep/ img {
  max-width: 100%;
}
</style>
