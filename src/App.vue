<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import liff from '@line/liff'
import { onMounted, ref } from 'vue';

const mainProfile = ref({})
onMounted(() => {
  liff
    .init({ liffId: '2004824985-x3R0k5ow' })
    .then(() => {
      // if (!liff.isLoggedIn()) {
      //   console.log('not login');
      //   liff.login();
      // } else {
      //   liff.getProfile().then((profile) => {
      //     console.log('displayName', profile.displayName);
      //     console.log('userId', profile.userId);
      //     console.log('pictureUrl', profile.pictureUrl);
      //     console.log('statusMessage', profile.statusMessage);
      //   });
      // }
      liff.getProfile().then((profile) => mainProfile.value = profile);
      console.log(liff.getOS())
      console.log(liff.getLanguage())
      console.log(liff.getVersion())
    })
    .catch((err) => {
      console.log(err.code, err.message);
    });
})

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld :msg="mainProfile" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
