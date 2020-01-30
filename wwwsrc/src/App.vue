<template>
  <div id="app">
    <button @click="login" v-if="!$auth.isAuthenticated">Login</button>
    <button @click="logout" v-else>logout</button>
    <h2>User</h2>
    <div>
      {{ $auth.user }}
    </div>
    <br />
    <br />
    <h2>User Info</h2>
    <div>
      {{ $auth.userInfo }}
    </div>
    <br />
    <br />
    <h2>Identity</h2>
    <div>
      {{ $auth.identity }}
    </div>
    <button @click="getPrivate">GetPrivate</button>
    {{ privateInfo }}
    <router-view />
  </div>
</template>

<script>
import axios from "axios";

let _api = axios.create({
  baseURL: "https://localhost:5001",
  withCredentials: true
});
export default {
  name: "App",
  data() {
    return {
      privateInfo: ""
    };
  },

  methods: {
    async login() {
      await this.$auth.loginWithPopup();
      console.log(this.$auth.user);
    },
    async logout() {
      await this.$auth.logout();
    },
    async getPrivate() {
      let res = await _api.get("WeatherForecast/private", {
        headers: {
          authorization: this.$auth.bearer
        }
      });
      this.privateInfo = res.data;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
