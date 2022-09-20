<template>
    <v-container class="welcome" fluid>
        <v-row align="center"
            justify="center">
              <h1>Welcome to Microsoft, you have <u>{{countDownDate ? countDownDate : -1}}</u> days until your start date</h1>
        </v-row>
    </v-container>
</template>

<script>
export default {
  name: 'Countdown',
  props: ['countDownDate'],
   created() {
  },
  data() {
    return {
    };
  }
}
</script>

<style scoped>

</style>
