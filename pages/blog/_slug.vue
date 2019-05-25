<template>
  <div>
    <section>
      <v-parallax
        :src="post.fields.heroImage.fields.file.url"
        height="400"
        class="hidden-sm-and-down"
      ></v-parallax>
    </section>
    <section>
      <v-container>
        <v-layout justify-center align-center>
          <v-flex xs12 lg8 class="px-5">
            <div>{{ post.fields.publishDate | formatDate }}</div>
            <h1 class="deep-purple--text text--darken-4">
              {{ post.fields.title }}
            </h1>
            <v-chip v-for="(tag, index) in post.fields.tags" :key="index"
              ># {{ tag }}</v-chip
            >
            <div>
              <VueMarkdown>{{ post.fields.body }}</VueMarkdown>
            </div>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
    <section class="pb-5">
      <div class="mb-5">
        <h3 class="mt-4 text-xs-center">Read more</h3>
        <v-divider class="my-4"></v-divider>
        <ArticlePreview :more="more" />
      </div>
    </section>
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'
import ArticlePreview from '~/components/article-preview.vue'
import { createClient } from '~/plugins/contentful.js'
import moment from 'moment'
import _ from 'lodash'

const client = createClient()
export default {
  components: {
    VueMarkdown,
    ArticlePreview
  },
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  asyncData({ env, params }) {
    return Promise.all([
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        'fields.slug': params.slug
      }),
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ])
      .then(([entries, posts]) => {
        let shuffle = _.shuffle(posts.items)
        return {
          post: entries.items[0],
          more: _.dropRight(shuffle, 2)
        }
      })
      .catch(console.error)
  }
}
</script>
