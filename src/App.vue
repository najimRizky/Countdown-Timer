<template>
  <v-app>
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>

<style>
  body{
  }
  #app{
    height: 100%;
    background-color: rgb(12, 39, 43);
  }
</style>
