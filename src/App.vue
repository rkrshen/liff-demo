<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Order from './components/Order.vue'
import liff from '@line/liff'
import { onMounted, ref } from 'vue';


let context:any = null
let contextKey:string[]

onMounted(() => {
  liff
    .init({ liffId: '2004827358-P3mE3j8E' })
    .then(() => {
      if (!liff.isLoggedIn()) {
        console.log('not login');
        // liff.login();
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
        context = liff.getContext()
        contextKey = Object.keys(context)
      }
    })
    .catch((err) => {
      console.log(err.code, err.message);
    });
})

const logout = () =>{
  liff.logout();
  liff.closeWindow();
}

</script>

<template>
  <div id="app">
    <Order />
    <a @click="logout">登出</a>
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
  color: #fff;
}
</style>
