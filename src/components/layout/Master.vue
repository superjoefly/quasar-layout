<template>
  <!-- Configure "view" prop for QLayout -->
  <q-layout ref="layout" v-touch-swipe.horizontal="swiped">
    <q-toolbar slot="header">
      <!-- opens drawer below -->
      <q-btn flat @click="$refs.layout.toggleLeft()">
        <i>menu</i>
      </q-btn>

      <q-toolbar-title>
        Quasar Layout
      </q-toolbar-title>
    </q-toolbar>

    <!-- Navigation Tabs -->
    <q-tabs slot="navigation">
      <q-route-tab slot="title" icon="home" to="/home" label="Home" replace />
      <q-route-tab slot="title" icon="book" to="/about" hide="icon" label="About" replace />
      <q-route-tab slot="title" icon="phone" to="/contact" hide="icon" label="Contact" replace />
      <q-route-tab slot="title" icon="list" to="/more" label="More" replace />
    </q-tabs>


    <!-- Left Side Panel -->
    <div slot="left">
      <q-list no-border link inset-delimiter>
        <q-list-header>Essential Links</q-list-header>
        <q-item>
          <q-item-side icon="school" />
          <q-item-main label="Docs" sublabel="quasar-framework.org" />
        </q-item>
        <q-item>
          <q-item-side icon="record_voice_over" />
          <q-item-main label="Forum" sublabel="forum.quasar-framework.org" />
        </q-item>
        <q-item>
          <q-item-side icon="chat" />
          <q-item-main label="Gitter Channel" sublabel="Quasar Lobby" />
        </q-item>
        <q-item>
          <q-item-side icon="rss feed" />
          <q-item-main label="Twitter" sublabel="@quasarframework" />
        </q-item>
      </q-list>
    </div>


    <!-- Right Side Panel -->
    <div slot="right">
      <p>Right Panel</p>
    </div>


    <router-view  />

    <!-- Footer -->
    <q-toolbar slot="footer">
      <p>Footer</p>
    </q-toolbar>

  </q-layout>
</template>

<script>
/*eslint-disable*/
import { QLayout, QToolbar, QToolbarTitle, QTabs, QRouteTab, QList, QListHeader, QItem, QItemSide, QItemMain, QBtn, TouchSwipe } from 'quasar'

export default {
  components: {
    QLayout, QToolbar, QToolbarTitle, QTabs, QRouteTab, QList, QListHeader, QItem, QItemSide, QItemMain, QBtn
  },
  directives: {
    TouchSwipe
  },
  data () {
    return {}
  },
  computed: {
    currentPath () {
      return this.$route.path
    },
    // routeList () {
    //   return this.$router.options.routes.map((route) => route.path )}
  },
  methods: {
      swiped (obj) {
        let path = this.currentPath
        let goto = function () {
          if (obj.direction == 'right') {
            if (path === '/home') {
              return
            } else if (path === '/about') {
              return '/home'
            } else if (path === '/contact') {
              return '/about'
            } else {
              return '/contact'
            }
          } else if (obj.direction == 'left') {
            if (path === '/home') {
              return '/about'
            } else if (path === '/about') {
              return '/contact'
            } else if (path === '/contact') {
              return '/more'
            } else {
              return
            }
          }
        }
        // If swipe is less than half window width, return
        if (Math.abs(obj.distance.x) < 0.5 * window.innerWidth) {
          return
        }
        // If goto returns a path, push to the router
        if (goto()) {
          this.$router.push(goto())
        }
      } // end swiped()


    // Another way using switch statement for delta
    // Determine direction of swipe
    // swiped (obj) {
    //   let path = this.currentPath
    //   let delta;
    //
    //   switch (obj.direction) {
    //     case 'right':
    //       delta = -1
    //       break
    //     case 'left':
    //       delta = 1
    //       break
    //     default:
    //       return
    //   }
    //
    //   // If swipe is less than half window, just return
    //   if (Math.abs(obj.distance.x) < 0.5 * window.innerWidth) {
    //     return
    //   }
    //
    //   this.routeList.getNext = function(delta, path) {
    //     if (delta == -1) {
    //       if (path == '/home') {
    //         return
    //       } else if (path == '/about') {
    //         return '/home'
    //       } else if (path == '/contact') {
    //         return '/about'
    //       } else {
    //         return '/contact'
    //       }
    //     }
    //     if (delta == 1) {
    //       if (path == '/home') {
    //         return '/about'
    //       } else if (path == '/about') {
    //         return '/contact'
    //       } else if (path == '/contact') {
    //         return '/more'
    //       } else {
    //         return
    //       }
    //     }
    //   }
    //
    //   let goto = this.routeList.getNext(delta, path)
    //   if (goto) {
    //     this.$router.push(goto)
    //   }
    // }
  }
}
</script>

<style>
</style>
