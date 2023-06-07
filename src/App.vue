<script setup>
import { ref, watch, onMounted } from "vue";
import { RouterLink, RouterView, useRoute } from "vue-router";

const pathName = ref("/");
const currentTab = ref(undefined);
const route = useRoute();
const drawer = ref(false);

const links = [
  {
    name: "WORK",
    route: "/",
  },
  {
    name: "ABOUT",
    route: "/about",
  },
  {
    name: "RESUME",
    href: "https://carolyn-yu.com/assets/doc/Resume.pdf",
  },
];

watch(
  () => route.path,
  async (to, from) => {
    // console.log(to, from);
    pathName.value = to;
  }
);

watch(currentTab, (to, from) => {
  if (to === 2) {
    currentTab.value = from;
  }
});
</script>

<template>
  <!--  :color=" '/' === pathName ? 'grey-lighten-4' : 'rgb(255,255,255,0.8)'"  -->
  <v-app class="bg-grey-lighten-5">
    <v-app-bar flat class="bg-grey-lighten-5">
      <v-container class="d-flex align-center">

      <v-app-bar-title>
        <RouterLink to="/" class="text-grey-darken-3">
          <strong>CAROLYN YU</strong>
        </RouterLink>
      </v-app-bar-title>
      <v-tabs v-model="currentTab" class="hidden-sm-and-down">
        <!-- <RouterLink
            to="/"
            :class="
              '/' !== pathName
                ? 'text-grey-darken-3'
                : 'text-pink-lighten-3 text-decoration-underline'
            "
          >
          <v-tab>Work</v-tab>
          </RouterLink>
          <RouterLink
            to="/about"
            :class="
              '/about' !== pathName
                ? 'text-grey-darken-3'
                : 'text-pink-lighten-3 text-decoration-underline'
            "
            >
          <v-tab>About</v-tab>
          </RouterLink>
          <v-btn><a class="text-grey-darken-3" href="@/assets/Resume.pdf" target="_blank">Resume</a></v-btn>
          <v-tab>
          <a
              class="text-grey-darken-3"
              href="https://carolyn-yu.com/assets/doc/Resume.pdf"
              target="_blank"
            >
              Resume
            </a> -->

        <v-tab
          v-for="{ name, route, href } in links"
          :key="name"
          :to="route"
          :href="href"
          :target="href ? '_blank' : undefined"
        >
          {{ name }}
        </v-tab>
      </v-tabs>

      <v-app-bar-nav-icon
        class="hidden-md-and-up"
        @click.stop="drawer = !drawer"
      ></v-app-bar-nav-icon>

      </v-container>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      :elevation="0"
      location="top"
      temporary
    >
      <v-list-item
        v-for="{ name, route, href } in links"
        :key="name"
        :to="route"
        :href="href"
        :target="href ? '_blank' : undefined"
        @click="drawer = false"
      >
        <v-list-item-title>{{ name }}</v-list-item-title>
      </v-list-item>
    </v-navigation-drawer>

    <v-main>
      <RouterView />
    </v-main>

    <v-footer
      class="text-center d-flex flex-column pb-6 pt-4 bg-grey-lighten-5"
    >
      <div class="text-grey">
        <p>
          &copy; Carolyn Yu {{ new Date().getFullYear() }} All rights reserved.
        </p>
      </div>
    </v-footer>
  </v-app>
</template>

<style lang="scss" scoped>
// v-app-bar
.v-toolbar__content {
  a {
    text-decoration: none;
  }
}

</style>
