<template>
  <v-app>
    <v-toolbar :clipped-left="clipped" fixed app dark flat class="app_nav px-3">
      <v-spacer />
      <v-toolbar-items>
        <v-btn
          flat
          icon
          to="/"
          class="mx-4"
          active-class="default-class no_active"
        >
          <v-icon large>fas fa-cannabis</v-icon>
        </v-btn>
        <v-btn
          flat
          icon
          to="/blog"
          class="mr-5"
          active-class="default-class no_active"
        >
          <v-icon class="mx-3" large>fas fa-book-dead</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-content>
      <nuxt />
    </v-content>
    <v-footer :fixed="fixed" app>
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: true,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'web',
          title: 'All thoughts',
          to: '/blog/'
        }
      ],
      miniVariant: false,
      right: true,
      title: 'High Thougts'
    }
  }
}
</script>
