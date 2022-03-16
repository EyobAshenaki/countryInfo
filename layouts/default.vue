<template>
  <v-app>
    <v-app-bar
      dark
      clipped-left
      elevate-on-scroll
      class="elevation-3"
      fixed
      app
    >
      <v-app-bar-nav-icon v-show="!right" @click.stop="drawer = !drawer" />
      <!-- <v-btn icon @click.stop="fixed = !fixed">
        <v-icon>mdi-minus</v-icon>
      </v-btn> -->
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn v-show="right" icon @click.stop="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :right="right"
      absolute
      clipped
      app
    >
      <v-list class="mt-md-14 mt-lg-0 pt-2">
        <v-list-item class="px-2" @click.native="right = !right">
          <v-list-item-title
            class="d-flex justify-space-between"
            :class="
              miniVariant
                ? 'flex-column pb-2'
                : right
                ? 'flex-row-reverse'
                : 'flex-row'
            "
          >
            <v-icon :class="miniVariant ? 'pb-1 pt-3' : ''">
              mdi-page-layout-sidebar-{{ right ? 'right' : 'left' }}
            </v-icon>
            <v-btn icon @click.stop="miniVariant = !miniVariant">
              <v-icon v-if="!right"
                >mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon
              >
              <v-icon v-else
                >mdi-{{ `chevron-${miniVariant ? 'left' : 'right'}` }}</v-icon
              >
            </v-btn>
          </v-list-item-title>
        </v-list-item>

        <v-divider class="mb-4"></v-divider>

        <v-list-item
          to="/"
          router
          exact
          @mouseenter="changeMapPicToGif"
          @mouseleave="changeMapGifToPic"
        >
          <v-list-item-action>
            <img
              v-if="!isMapIconGif"
              class="pr-1"
              src="../assets/img/map/map.png"
              style="max-height: 1.9em; max-width: 1.9em"
            />
            <img
              v-else
              class="pa-0 ma-0"
              src="../assets/img/map/map.gif"
              style="max-height: 1.65em; max-width: 1.65em"
            />
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title> Countires </v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item
          to="/weather"
          router
          exact
          @mouseenter="changeWeatherPicToGif"
          @mouseleave="changeWeatherGifToPic"
        >
          <v-list-item-action>
            <img
              v-if="!isWeatherIconGif"
              class="pr-1"
              src="../assets/img/cloudy/cloudy.png"
              style="max-height: 1.9em; max-width: 1.9em"
            />
            <img
              v-else
              class="pa-0 ma-0"
              src="../assets/img/cloudy/cloudy.gif"
              style="max-height: 1.65em; max-width: 1.65em"
            />
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title> Weather </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="mt-5">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <!-- <v-footer class="justify-center" dark fixed app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer> -->
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',

  data() {
    return {
      drawer: false,
      miniVariant: false,
      right: false,
      title: 'CountryInfo',
      isMapIconGif: false,
      isWeatherIconGif: false,
    }
  },
  methods: {
    changeMapPicToGif() {
      this.isMapIconGif = true
    },
    changeMapGifToPic() {
      this.isMapIconGif = false
    },
    changeWeatherPicToGif() {
      this.isWeatherIconGif = true
    },
    changeWeatherGifToPic() {
      this.isWeatherIconGif = false
    },
  },
}
</script>
