<template>
  <v-layout justify-center>
    <v-flex v-for="(post, index) in more" :key="index" lg3 pa-2 class="mt-3">
      <a :href="/blog/ + post.fields.slug">
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
</template>

<script>
import moment from 'moment'

export default {
  filters: {
    formatDate(val) {
      let date = moment(val).format('MMM Do, YYYY')
      return date
    }
  },
  props: {
    more: {
      type: Array,
      default: () => []
    }
  }
}
</script>
