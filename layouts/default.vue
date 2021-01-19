<template>
  <v-app>
    <Navbar v-if="!['detail-detail'].includes($route.name)" />

    <nuxt />
  </v-app>
</template>

<script>
import Navbar from "../components/Navigation/Navbar.vue";
export default {
  components: { Navbar }
};
</script>
