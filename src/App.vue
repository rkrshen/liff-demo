<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import liff from '@line/liff'
import { onMounted } from 'vue';

onMounted(() => {
  liff
    .init({ liffId: '2004827358-P3mE3j8E' })
    .then(() => {
      if (!liff.isLoggedIn()) {
        console.log('not login');
        liff.login();
      } else {
        console.log(liff.getOS())
        console.log(liff.getLanguage())
        console.log(liff.getVersion())
        liff.getProfile().then((profile) => {
          console.log(profile.displayName);
          console.log(profile.userId);
          console.log(profile.pictureUrl);
          console.log(profile.statusMessage);
        });
      }
    })
    .catch((err) => {
      console.log(err.code, err.message);
    });
})

</script>

<template>
  <div id="app"></div>
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
    <RouterLink to="/about">About</RouterLink>
    <RouterView />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
