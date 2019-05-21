<template>
  <div>
    <section>
      <v-parallax class="hidden-sm-and-down" :src="hero"></v-parallax>
    </section>
    <Intro />
    <section>
      <v-container grid-list-sm>
        <v-layout>
          <v-flex>
            <h3 class="text-xs-center font-weight-thin">Recent thoughts</h3>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>

        <v-layout row wrap class="mt-5">
          <v-flex v-for="(post, index) in posts" :key="index" lg3 pa-2>
            <a v-if="index <= 5" :href="/blog/ + post.fields.slug">
              <v-card hover ripple>
                <v-img
                  :src="post.fields.heroImage.fields.file.url"
                  max-height="300"
                ></v-img>
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
            </a>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import moment from 'moment'
import Intro from '~/components/Intro'

const client = createClient()

export default {
  components: {
    Intro
  },
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  // `env` is available in the context object
  asyncData({ env }) {
    return Promise.all([
      client.getEntries({
        content_type: 'hero'
      }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ])
      .then(([entries, posts]) => {
        // return data that should be available
        // in the template
        return {
          posts: posts.items,
          hero: entries.items[0].fields.heroImage.fields.file.url
        }
      })
      .catch(console.error)
  }
}
</script>
