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
          <v-btn value="Trending">
            <v-icon>mdi-trending-up</v-icon>
          </v-btn>
          <v-btn value="Top">
            <v-icon>mdi-arrow-up-bold-box-outline</v-icon>
          </v-btn>
          <v-btn value="New">
            <v-icon>mdi-decagram-outline</v-icon>
          </v-btn>
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
          <v-list-item :key="item">
            <v-list-item-action>
              <v-btn
                fab
                small
                depressed
                color="primary"
              >
                <v-icon>mdi-account</v-icon>
              </v-btn>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title>
                Question <strong>ID {{ item }}</strong>
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-icon small>
                mdi-open-in-new
              </v-icon>
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
export default {
    name: 'Feed',
    props: {
    },
    computed: {
      items () {
        return Array.from({ length: this.length }, (k, v) => v + 1);
      },
      length () {
        return Math.floor(Math.random() * 20);
      },
    },
    data() {
    return {
        feedType: "Trending",
        isLoading: false
    };
  },
  methods: {
    loadContent() {
            this.isLoading = !this.isLoading;
        setTimeout(()=> {
            this.isLoading = !this.isLoading;
        }, 200)

    }
  }
}
</script>

<style scoped>

</style>