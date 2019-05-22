<template>
  <div>
    <section>
      <v-parallax
        :src="post.fields.heroImage.fields.file.url"
        height="300"
        class="hidden-sm-and-down"
      ></v-parallax>
    </section>
    <section>
      <v-container>
        <v-layout justify-center align-center>
          <v-flex xs8>
            <div>{{ post.fields.publishDate | formatDate }}</div>
            <h1 class="deep-purple--text text--darken-4">
              {{ post.fields.title }}
            </h1>
            <v-chip v-for="(tag, index) in post.fields.tags" :key="index"
              ># {{ tag }}</v-chip
            >
            <v-card
              :img="
                post.fields.bodyImage
                  ? post.fields.bodyImage.fields.file.url
                  : ''
              "
              height="100%"
              class="mt-5"
            >
              <v-card-text class="pa-5 white--text gradient_bg">
                <VueMarkdown>{{ post.fields.body }}</VueMarkdown>
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'
import { createClient } from '~/plugins/contentful.js'
import moment from 'moment'
const client = createClient()
export default {
  components: {
    VueMarkdown
  },
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  asyncData({ env, params }) {
    return client
      .getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        'fields.slug': params.slug
      })
      .then(entries => {
        return {
          post: entries.items[0]
        }
      })
      .catch(console.error)
  }
}
</script>
