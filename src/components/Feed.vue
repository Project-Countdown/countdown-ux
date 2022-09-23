<template>
  <div class="question-feed">
    <v-card
      elevation="16"
      class="mx-auto"
    >
    <v-card-title>
        {{feedType}} Questions
        <v-spacer></v-spacer>
        <v-btn-toggle
          v-model="feedType"
          mandatory
          rounded
          @change="loadContent"
        >
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn
                    v-bind="attrs"
                    v-on="on"
                    value="Trending">
                    <v-icon>mdi-trending-up</v-icon>
                    </v-btn>
                </template>
                <span>Trending</span>
            </v-tooltip>
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn
                    v-bind="attrs"
                    v-on="on"
                    value="Top">
                    <v-icon>mdi-arrow-up-bold-box-outline</v-icon>
                    </v-btn>
                </template>
                <span>Top</span>
            </v-tooltip>
            <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn
                    v-bind="attrs"
                    v-on="on"
                    value="New">
                    <v-icon>mdi-decagram-outline</v-icon>
                    </v-btn>
                </template>
                <span>New</span>
            </v-tooltip>
        </v-btn-toggle>

    </v-card-title>
        <v-container v-if="isLoading">
            <v-progress-linear
            indeterminate
            color="primary"
            ></v-progress-linear>
      </v-container>
      <template v-else>
      <v-virtual-scroll
        bench="10"
        :items="items"
        height="60vh"
        max-height="60vh"
        item-height="75"
      >
        <template v-slot:default="{ item }">
          <v-list-item :key="item.fullName">
            <v-list-item-action>
                <v-tooltip left>
                     <template v-slot:activator="{ on }">
                        <v-btn
                                fab
                                small
                                depressed
                                color="primary"
                                v-on="on"
                            >
                            <v-icon>mdi-account</v-icon>
                         </v-btn>
                    </template>
                <span>{{ item.fullName }}</span>
                </v-tooltip>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title>
               {{ item.question }}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
                <QuestionDialog :question="item.question"/>
            </v-list-item-action>
          </v-list-item>

          <v-divider></v-divider>
        </template>
      </v-virtual-scroll>
      </template>
    </v-card>
  </div>
</template>

<script>
import { faker } from "@faker-js/faker";
import QuestionDialog from "./QuestionDialog.vue";

export default {
    name: 'Feed',
    components: {
        QuestionDialog
    },
    props: {
        questionInput: String
    },
    computed: {
      items () {
        return Array.from({ length: this.length }, () => { 
            return { 
                fullName: faker.name.fullName(),
                question: faker.hacker.phrase()
            }});
      },
    },
    data() {
    return {
        feedType: "Trending",
        isLoading: false,
        length: Math.floor(Math.random() * 30) + 1,
        faker: faker,
    };
  },
  methods: {
    loadContent() {
            this.isLoading = !this.isLoading;
        setTimeout(()=> {
            this.isLoading = !this.isLoading;
            this.length = Math.floor(Math.random() * 30) + 1;
        }, Math.floor(Math.random() * 600) + 100)

    },
    async getQuestions(){
      const requestOptions = {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({
          PartitionKey: "",
          RowKey: "",
          RequestType: "all",
          Text: ""
        })
      };
      const response = await fetch(
        "https://prod-41.eastus.logic.azure.com:443/workflows/42ee109f7da94a91bbc3c74388040e34/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=tUGgVy-E0N95ovXLZQvgbnsMC4OmPPQsJv3sJTt3ulI",
        requestOptions);
      const questions = await response.json();
      return questions['value'];
    }
  },
  watch: {
    questionInput() {
        this.loadContent();
    }
  }
}
</script>

<style scoped>

</style>