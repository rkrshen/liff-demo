<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Order from './components/Order.vue'
import liff from '@line/liff'
import { onMounted, ref } from 'vue';


let context:any = null
let contextKey:string[]

interface UserData {
    displayName: string
    pictureUrl: string
}
let userData:UserData = {
    displayName: '',
    pictureUrl: ''
}

onMounted(async() => {
  await liff
    .init({ liffId: '2004827358-P3mE3j8E' })
    .then(() => {
      if (!liff.isLoggedIn()) {
        console.log('not login');
        // liff.login();
      } 
    })
    .catch((err) => {
      console.log(err.code, err.message);
    });
  liff.getProfile()
    .then((prof) => {
      userData.displayName = prof.displayName
      userData.pictureUrl = prof.pictureUrl ?? ''
    })
    .catch((err) => {
      console.log(err.code, err.message);
    });
})
</script>

<template>
  <div id="app">
    <Order v-model="userData" />
    <table v-if="context !== null">
      <tr v-for="item in contextKey">
        <td>{{ item }}</td>
        <td>{{ context[item] }}</td>
      </tr>
    </table>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
