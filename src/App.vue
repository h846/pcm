<template>
  <v-app>
    <v-main class="bg">
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",
};
</script>
<style scoped>
.bg {
  background: url("~@/assets/bg.jpg") no-repeat center center fixed;
  background-size: cover;
}
</style>