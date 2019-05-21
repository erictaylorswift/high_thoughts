<template>
  <v-container>
    <v-layout class="my-4">
      <v-flex>
        <h1 class="text-xs-center page_heading">All my thoughts while high</h1>
        <p class="body-1 text-xs-center">(In random order)</p>
      </v-flex>
    </v-layout>
    <v-layout row wrap class="my-4">
      <v-flex v-for="(post, index) in posts" :key="post.id" xs6 pa-2>
        <v-card v-if="index <= 2">
          <v-img :src="post.fields.heroImage.fields.file.url"></v-img>
          <v-card-title>
            <v-layout>
              <v-flex>
                <h3 class="card-title deep-purple--text text--darken-4">
                  {{ post.fields.title }}
                </h3>
                <div class="caption">
                  {{ post.fields.publishDate | formatDate }}
                </div>
              </v-flex>
            </v-layout>
          </v-card-title>
          <v-card-text class="pt-0">
            <div class="text-truncate body-1">
              {{ post.fields.description }}
            </div>
          </v-card-text>
          <v-card-actions>
            <v-chip v-for="tag in post.fields.tags" :key="tag.id"
              ># {{ tag }}</v-chip
            >
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
    <v-divider></v-divider>
    <v-layout row wrap>
      <v-flex v-for="(post, index) in posts" :key="post.id" xs3 pa-2>
        <v-card v-if="index >= 2">
          <v-img :src="post.fields.heroImage.fields.file.url"></v-img>
          <v-card-title>
            <v-layout>
              <v-flex>
                <h3 class="card-title deep-purple--text text--darken-4">
                  {{ post.fields.title }}
                </h3>
                <div class="caption">
                  {{ post.fields.publishDate | formatDate }}
                </div>
              </v-flex>
            </v-layout>
          </v-card-title>
          <v-card-text class="pt-0">
            <div class="text-truncate body-1">
              {{ post.fields.description }}
            </div>
          </v-card-text>
          <v-card-actions>
            <v-chip v-for="tag in post.fields.tags" :key="tag.id"
              ># {{ tag }}</v-chip
            >
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import _ from 'lodash'
import moment from 'moment'

const client = createClient()
export default {
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
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
