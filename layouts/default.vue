<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-toolbar flat>
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <v-btn icon right @click.stop="miniVariant = !miniVariant">
            <v-icon>{{ `chevron_${miniVariant ? 'right' : 'left'}` }}</v-icon>
          </v-btn>
        </v-toolbar-items>
      </v-toolbar>
      <v-list>
        <v-list-tile
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title" />
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar :clipped-left="clipped" fixed app dark flat class="app_nav">
      <v-toolbar-side-icon @click="drawer = !drawer" />
      <font-awesome-icon
        :icon="['fas', 'cannabis']"
        style="font-size: 22px;"
        class="mr-0"
      />
      <v-btn flat nuxt to="/">{{ title }}</v-btn>
      <v-spacer />
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
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'apps',
          title: 'Welcome',
          to: '/'
        },
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
