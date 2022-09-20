<template>
  <v-app>
    <v-app-bar
      app
      class="m-2"
      elevation="0">
        <v-icon>mdi-rocket</v-icon>
        <v-toolbar-title>Project Countdown</v-toolbar-title>
        <v-spacer/>
        <v-btn
          elevation="2"
          fab
        >{{countDownDate}}</v-btn>
    </v-app-bar>

    <v-main>
      <Countdown :countDownDate="countDownDate"></Countdown>
      <MainSubmit/>
      <Feed/>
    </v-main>
  </v-app>
</template>

<script>
import MainSubmit from './components/MainSubmit';
import Countdown from './components/Countdown';
import Feed from './components/Feed';
export default {
  name: 'App',

  components: {
    MainSubmit,
    Countdown,
    Feed
  },
  computed: {
    countDownDate: () => {
      return Math.floor(Math.random() * 100) + 10;
    }
  },
  data: () => ({
  }),
};
</script>
