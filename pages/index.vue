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

        <ArticlePreview :more="onlyFive" />
      </v-container>
    </section>
  </div>
</template>

<script>
import ArticlePreview from '~/components/article-preview.vue'
import { createClient } from '~/plugins/contentful.js'
import moment from 'moment'
import Intro from '~/components/Intro'

const client = createClient()

export default {
  components: {
    Intro,
    ArticlePreview
  },
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  computed: {
    onlyFive() {
      let maxFiveArray = []

      for (var i = 0; i < 6; i++) {
        if (this.posts[i]) {
          maxFiveArray.push(this.posts[i])
        }
      }

      return maxFiveArray
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
        return {
          posts: posts.items,
          hero: entries.items[0].fields.heroImage.fields.file.url
        }
      })
      .catch(console.error)
  }
}
</script>
