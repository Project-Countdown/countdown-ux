<template>
  <v-container class="welcome my-2" fluid>
        <v-row align="center"
            justify="center"
            class="mx-5">
            <v-autocomplete
              v-model="searchInput"
              :items="items"
              :loading="isLoading"
              :search-input.sync="search"
              :hide-no-data="search == null"
              hide-details
              clearable
              hide-selected
              item-text="Description"
              item-value="API"
              label="Ask a question"
              placeholder="What can I expect on my first day?"
              solo
              @update:search-input="handleChange"
              @input="emitQuestionInput"
            >
            <template slot="no-data" class="mx-2">
              <v-btn
                color="primary"
                block
                outlined
                rounded
                :disabled="buttonDisabled"
                :loading="buttonLoading"
                elevation="0"
                @click="askQuestion()">
                {{ askText }}
                <v-icon>{{ askIcon }}</v-icon> {{ askEmail }}
              </v-btn>
            </template>
            </v-autocomplete>
            
        </v-row>
    </v-container>
</template>

<script>
export default {
  name: 'MainSubmit',
  props: {
  },
  data () {
    return {
      searchInput: null,
      search: null,
      isLoading: false,
      items: [],
      showAskButton: false,
      askText: "Ask",
      askIcon: "mdi-chat-question",
      askEmail: "",
      buttonLoading: false,
      buttonDisabled: false,
    }
  },
  methods: {
    handleChange(searchWord) {
      if (this.buttonDisabled){
        this.buttonDisabled = false;
      }
      if (this.buttonLoading){
        this.buttonLoading = false;
      }
      if(this.askText !== "Ask"){
         this.askText = "Ask";
         this.askEmail = "";
      }
      if (this.askIcon !== "mdi-chat-question"){
        this.askIcon = "mdi-chat-question";
      }
      if (this.items.filter(value => value.startsWith(searchWord)).length === 0) {
        this.showAskButton = true;
      }
      else {
        this.showAskButton = false;
      }
    },
    emitQuestionInput(question) {
      this.$emit('questionInputEvent', question);
    },
    emitRefreshFeed(){
      this.$emit('refreshFeedEvent');
    },
    async askQuestion (){
      this.buttonLoading = true;
      this.buttonDisabled = true;
      const year = new Date().getFullYear();
      const requestOptions = {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({
          PartitionKey: year.toString(),
          RowKey: "",
          RequestType: "add",
          Text: this.search
        })
      };
      //comment-check-outline
      const response = await fetch(
        "https://prod-41.eastus.logic.azure.com:443/workflows/42ee109f7da94a91bbc3c74388040e34/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=tUGgVy-E0N95ovXLZQvgbnsMC4OmPPQsJv3sJTt3ulI",
        requestOptions);
      const question = await response.json();
      if (response['status'] === 200){
        console.log("Made it");
        console.log(question);
        this.buttonLoading = false;
        this.askText = "Question Submitted";
        this.askIcon = "mdi-comment-check-outline";
        this.askEmail = " to " + question["email"];
        this.emitRefreshFeed();
      }
      return question;
    }
  },
  watch: {
    search () {
        // Items have already been loaded
        if (this.items.length > 0) return

        // Items have already been requested
        if (this.isLoading) return

        this.isLoading = true

        //API Request here
        setTimeout(() => {
          this.items = [
            "What can I expect on my first day?",
            'Where are the bathrooms?',
            'What do I wear?',
            'Can I bike to work?',
            'How much is my signing bonus taxed?',
            'What is life',
            'Are we there yet?',
            'How much is the average cost of lunch in Reston?',
            'How much is the average cost of lunch in Redmond?',
            'What are the benefits?',
            'Are there microwaves in Reston?',
          ]
          this.isLoading = false;
        }, 100);

       
      },
    },
}
</script>

<style scoped>

</style>