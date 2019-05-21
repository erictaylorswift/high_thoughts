<template>
  <div>
    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>All articles ({{ posts.length }})</h2>
      </div>
      <ul class="items-list wrapper">
        <li v-for="post in posts" :key="post.id" class="item">
          {{ post.fields.title }}
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import _ from 'lodash'

const client = createClient()
export default {
  asyncData({ env, params }) {
    return client
      .getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID
      })
      .then(entries => {
        return {
          posts: _.shuffle(entries.items),
          'fields.slug': params.slug
        }
      })
  }
}
</script>
