<template>
    <v-container fluid>
        <v-dialog
            v-model="dialog"
            width="75vw"
            >
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                    icon
                    depressed
                    small
                    v-bind="attrs"
                    v-on="on">
                    <v-icon small>
                        mdi-open-in-new
                    </v-icon>
                 </v-btn>
            </template>

            <v-card>
                <v-card-title class="text-h5 grey lighten-2">
                    {{question}}
                    <v-spacer></v-spacer>
                    <v-btn
                        icon
                        @click="dialog = false">
                        <v-icon>mdi-close</v-icon>
                    </v-btn>
                </v-card-title>

                <v-card-text>
                   <v-row class="my-5">
                        <v-icon>mdi-account</v-icon>
                        <v-chip>Quis beatae quis corporis veritatis corrupti ratione delectus sapiente ut.</v-chip>
                   </v-row>
                   <v-row class="my-5">
                        <v-icon>mdi-account</v-icon>
                        <v-chip>Dolor tempora iusto.</v-chip>
                   </v-row>
                   <v-row class="my-5">
                        <v-icon>mdi-account</v-icon>
                        <v-chip>Rerum eum reiciendis id ipsa hic dolore aut laborum provident</v-chip>
                   </v-row>
                </v-card-text>
            </v-card>
        </v-dialog>
    </v-container>
</template>

<script>

export default {
  name: 'QuestionDialog',
  props: {
    question: String,
  },
  mounted() {
  },
  computed: {
  },
  data() {
    return {
        dialog: false,
    };
  },
  methods: {
  }
}
</script>