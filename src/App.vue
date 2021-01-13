<template>
  <v-app class="bg">
    <v-app-bar flat app color="primary">
      <v-toolbar flat color="primary">
        <router-link to="/" style="text-decoration: none; color: inherit;">
        <v-toolbar-title class="white--text"><v-icon medium dense color="white">mdi-umbrella-outline</v-icon> Umbrello.</v-toolbar-title>
        </router-link>
        <v-spacer></v-spacer>
        <v-btn color="primary" elevation="0"
        v-if="accessToken==null" to="/login">
          Sign In
        </v-btn>
        <v-btn color="primary" elevation="0"
        v-if="accessToken==null" to="/register">
          Sign Up
        </v-btn>
        <v-btn color="primary" elevation="0"
        v-if="accessToken!=null" to="/dashboard">
          Boards
        </v-btn>
        <v-btn color="secondary" elevation="0"
        v-if="accessToken!=null" to="/logout">
          Sign Out
        </v-btn>
      </v-toolbar>
    </v-app-bar>
    <v-main class="bg">
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
  import { mapState } from 'vuex'
  export default {
    name: 'App',
    computed: mapState(['accessToken'])
  }
</script>

<style>
html {
  overflow: overlay;
}

html::-webkit-scrollbar {
  width: 15px;
}

html::-webkit-scrollbar-track {
  background: #202020;
  border-left: 1px solid #2c2c2c;
}

html::-webkit-scrollbar-thumb {
  background: #3e3e3e;
  border: solid 3px #202020;
  border-radius: 7px;
}

html::-webkit-scrollbar-thumb:hover {
  background: white;
}

.bg {
    background: url('https://i.imgur.com/C8BYhgw.jpeg') no-repeat center center;
    background-size: fill;
  }
</style>
