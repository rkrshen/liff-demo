<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import liff from '@line/liff'
import { onMounted, ref } from 'vue';

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

let context:any = null
let contextKey:string[]

const errorMsg = ref('')
const sendMsg = () =>{
  liff.sendMessages([
    {
      type: 'text',
      text: 'Hello, World!',
    },
  ])
  .then(() => {
    alert('message sent');
  })
  .catch((err) => {
    errorMsg.value= err
  });
}
const logout = () =>{
  liff.logout();
  liff.closeWindow();
}

</script>

<template>
  <div id="app"></div>
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
    <a @click="sendMsg">點我傳送訊息</a>
    <a @click="logout">登出</a>
    <span v-if="errorMsg">{{ errorMsg }}</span>
    <table>
      <tr v-for="item in contextKey">
        <td>{{ item }}</td>
        <td>{{ context[item] }}</td>
      </tr>
    </table>
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
