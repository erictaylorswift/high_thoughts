<template>
  <div>
    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>All articles tagged #{{ tag }} ({{ posts.length }})</h2>
      </div>
    </section>
    <ArticlePreview :more="posts" />
  </div>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import ArticlePreview from '~/components/article-preview.vue'

const client = createClient()
export default {
  components: {
    ArticlePreview
  },
  data() {
    return {
      tagSlug: this.$route.params.tags
    }
  },
  asyncData({ env, params }) {
    return client
      .getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        'fields.tags[in]': params.tags,
        order: '-sys.createdAt'
      })
      .then(entries => {
        return {
          posts: entries.items,
          tag: params.tags
        }
      })
  }
}
</script>
