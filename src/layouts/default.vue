<template>
  <v-app>
    <v-layout>
      <v-navigation-drawer v-model="drawer" color="#040410">
        <v-container class="d-flex justify-center align-center" fluid>
          <v-img :src="logo" class="circular-image" width="200px"></v-img>
        </v-container>

        <v-list item-props :items="items" nav />

        <template #append>
          <v-list-item class="ma-2" link nav prepend-icon="mdi-cog-outline" title="Settings" />
        </template>
      </v-navigation-drawer>

      <v-app-bar border="b" class="ps-4" flat color="#b064fc">
        <v-app-bar-nav-icon v-if="$vuetify.display.smAndDown" @click="drawer = !drawer" />

        <v-app-bar-title class="lobster-font text-h4">Practice Japanese</v-app-bar-title>

        <template #append>
          <v-btn class="text-none me-2" height="48" icon slim>
            <v-avatar color="surface-light" image="https://cdn.vuetifyjs.com/images/john.png" size="32" />

            <v-menu activator="parent">
              <v-list density="compact" nav>
                <v-list-item append-icon="mdi-cog-outline" link title="Settings" />

                <v-list-item append-icon="mdi-logout" link title="Logout" />
              </v-list>
            </v-menu>
          </v-btn>
        </template>
      </v-app-bar>

      <v-main>
        <router-view />
      </v-main>
    </v-layout>
  </v-app>

</template>

<script setup>
import { ref } from 'vue'
import logo from '@/assets/logo.jpg'

const drawer = ref(true)

const items = ref([
  {
    title: 'Home',
    prependIcon: 'mdi-home-outline',
    link: true,
  },
  {
    title: 'Learn',
    prependIcon: 'mdi-school-outline',
    link: true,
  },
  {
    title: 'Practice',
    prependIcon: 'mdi-briefcase-outline',
    link: true,
  },
  {
    title: 'Reports',
    prependIcon: 'mdi-file-chart-outline',
    link: true,
  },
])
</script>

<style>
.circular-image {
  border-radius: 70%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.lobster-font {
  font-family: 'Nanum Brush Script', sans-serif;
}
</style>