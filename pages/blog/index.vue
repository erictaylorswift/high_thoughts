<template>
  <div>
    <header class="blog header">
      <div class="foreground">
        <div class="page-bar wrapper">
          <a href="/" class="person-name">John Doe</a>
        </div>
        <div class="page-info wrapper">
          <h2>Blog</h2>
        </div>
      </div>
    </header>

    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>All articles ({{ posts.length }})</h2>
      </div>
      <ul class="items-list wrapper">
        <li v-for="post in posts" :key="post.id" class="item">
          <ArticlePreview :post="post"></ArticlePreview>
        </li>
      </ul>
    </section>
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
  asyncData({ env, params }) {
    return client
      .getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
      .then(entries => {
        return {
          posts: entries.items,
          'fields.slug': params.slug
        }
      })
  }
}
</script>
