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
              hide-no-data
              hide-details
              clearable
              hide-selected
              item-text="Description"
              item-value="API"
              label="Ask a question"
              placeholder="What can I expect on my first day?"
              prepend-icon="mdi-chat-question"
              solo
            ></v-autocomplete>
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
      isLoading: false,
      items: [
        'What do I wear?',
        'Can I bike to work?',
        'How much is my signing bonus taxed',
        'What is life',
        'Are we there yet?'
      ]
    }
  },
  watch: {
    },
}
</script>

<style scoped>

</style>