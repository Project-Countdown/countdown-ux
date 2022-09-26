<template>
  <v-app>
    <v-app-bar
      app
      class="m-2"
      elevation="0">
        <v-icon>mdi-rocket</v-icon>
        <v-toolbar-title>Project Countdown</v-toolbar-title>
        <v-spacer/>
        <p id="Days" class="mt-10"><span class='box'>{{countDownDate}}</span></p> 
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-card
          elevation="0"
          max-width="80vw"
          class="mx-auto"
        >
          <Countdown :countDownDate="countDownDate"/>
          <MainSubmit @questionInputEvent="(question) => questionInput = question"
            @refreshFeedEvent="refreshFeed"/>
          <Feed @refreshedFeed="refreshedFeed" :questionInput="questionInput"
          :refreshFeedProp="refreshFeedProp"/>
        </v-card>
      </v-container>
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
    questionInput: null,
    refreshFeedProp: false,
  }),
  methods: {
    refreshFeed : function(){
      console.log("emit");
      this.refreshFeedProp = true;
    },
    refreshedFeed : function () {
      console.log("set back");
      this.refreshFeedProp = false;
    }
  },
};
</script>

<style scoped>
.box:before{
    content: "";
    background: darkgrey;
    position: absolute;
    z-index: 10;
    top: 45px;
    left: 0;
    width: 100%;
    height: 2px;
    border-bottom: 1px solid #fff;
}
.box{
    margin:0 5px;
    padding: 10px;
    position: relative;
    /*float: left;*/
    width: 60px;
    height: 86px;
    font-size: 60px;
    line-height: 86px;
    border-radius: 4px;
    box-shadow: 0 3px 3px rgba(0,0,0,0.3);
    color:#555555;
  /* Permalink - use to edit and share this gradient: http://colorzilla.com/gradient-editor/#ebebeb+0,ebebeb+50,ffffff+51,f8f8f8+100 */
    background: rgb(235,235,235); /* Old browsers */
    background: -moz-linear-gradient(top, rgba(235,235,235,1) 0%, rgba(235,235,235,1) 50%, rgba(255,255,255,1) 51%, rgba(248,248,248,1) 100%); /* FF3.6-15 */
    background: -webkit-linear-gradient(top, rgba(235,235,235,1) 0%,rgba(235,235,235,1) 50%,rgba(255,255,255,1) 51%,rgba(248,248,248,1) 100%); /* Chrome10-25,Safari5.1-6 */
    background: linear-gradient(to bottom, rgba(235,235,235,1) 0%,rgba(235,235,235,1) 50%,rgba(255,255,255,1) 51%,rgba(248,248,248,1) 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#ebebeb', endColorstr='#f8f8f8',GradientType=0 ); /* IE6-9 */
}

#Days {
    margin:0px;
}
</style>