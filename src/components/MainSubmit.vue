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
                elevation="0">
                Ask
                <v-icon>mdi-chat-question</v-icon>
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
    }
  },
  methods: {
    handleChange(searchWord) {
      if (this.items.filter(value => value.startsWith(searchWord)).length === 0) {
        this.showAskButton = true;
      }
      else {
        this.showAskButton = false;
      }
    },
    emitQuestionInput(question) {
      this.$emit('questionInputEvent', question);
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