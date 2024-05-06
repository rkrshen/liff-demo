<template>
  <div v-if="!isLogin">
    <h1>create333-liff-app</h1>
    <p v-if="message">{{ message }}</p>
    <p v-if="error">
      <code>{{ error }}</code>
    </p>
    <a href="https://developers.line.biz/ja/docs/liff/" target="_blank" rel="noreferrer">
      LIFF Documentation
    </a>
  </div>
  <div v-else>


  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import liff from "@line/liff";

export default defineComponent({
  data() {
    return {
      message: "",
      error: "",
      isLogin: false,
      profile: {},
    };
  },
  mounted() {
    liff
      .init({
        liffId: "2004827358-P3mE3j8E",
        withLoginOnExternalBrowser: true,
      })
      .then(() => {
        this.message = "LIFF init succeeded.";
        if(liff.isLoggedIn()) {
          this.message += " You are logged in.";
          this.isLogin = true;
          this.profile = liff.getProfile();
        } else {
          this.message += " You are not logged in.";
          liff.login({ redirectUri: "https://localhost:5173" });
        }
      })
      .catch((e: Error) => {
        this.message = "LIFF init failed.";
        this.error = `${e}`;
      });
  }
});
</script>

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
