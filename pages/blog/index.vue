<template>
  <v-container>
    <div style="border-bottom: solid 1px;">
      <v-toolbar flat color="white" tabs>
        <v-toolbar-title>All tags</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items v-for="tag in tags" :key="tag">
          <v-btn flat :to="'tags/' + tag">{{ tag }}</v-btn>
        </v-toolbar-items>
      </v-toolbar>
    </div>
    <v-layout class="my-4" column justify-center align-center>
      <v-flex>
        <v-icon large class="center_icon">fas fa-cannabis</v-icon>
        <h1 class="text-xs-center page_heading">All my thoughts while high</h1>
      </v-flex>
    </v-layout>
    <v-layout row wrap class="my-4">
      <v-flex v-for="(post, index) in posts" :key="post.id" xs6 pa-2>
        <v-card v-if="index <= 1">
          <v-img :src="post.fields.heroImage.fields.file.url"></v-img>
          <v-card-title>
            <v-layout>
              <v-flex>
                <h3 class="card-title deep-purple--text text--darken-4">
                  <a :href="/blog/ + post.fields.slug">
                    {{ post.fields.title }}
                  </a>
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
    <ArticlePreview :more="more" />
  </v-container>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import ArticlePreview from '~/components/article-preview.vue'
import _ from 'lodash'
import moment from 'moment'

const client = createClient()
export default {
  components: {
    ArticlePreview
  },
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  computed: {
    tags() {
      let tags = []
      let posts = this.posts

      posts.forEach(post => {
        tags.push(post.fields.tags)
      })

      let flat = _.flattenDeep(tags)
      let flattened = []

      flat.forEach(t => {
        flattened.push(t.replace(/\s/g, '-'))
      })

      return _.uniq(flattened)
    }
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
          'fields.slug': params.slug,
          more: _.drop(entries.items, 2)
        }
      })
  }
}
</script>
