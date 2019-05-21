<template>
  <div>
    <section>
      <v-parallax
        :src="post.fields.heroImage.fields.file.url"
        height="300"
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
            <v-chip v-for="(tag, index) in post.fields.tags" :key="index">
              # {{ tag }}
            </v-chip>
            <VueMarkdown>{{ post.fields.body }}</VueMarkdown>
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

<style>
.foreground .page-bar {
  border-bottom: 0;
}
.headline {
  padding: 3em 0 0;
}
.headline h1 {
  font-size: 3.5em;
}
.copy {
  padding-bottom: 7em;
}
.copy *:not(div) {
  margin: 2em 0 1em;
}
.copy h3 {
  font-size: 1.35em;
}
.copy ul {
  margin: 0;
  padding-left: 1em;
  list-style: disc;
}
.copy li {
  margin: 0;
}
</style>
