<template>
<v-card min-width="100">
    <v-dialog
      v-model="cardDialog"
      max-width="600px"
    >
        <template v-slot:activator="{ on, attrs }">
            <v-card
            v-on="on"
            v-bind="attrs"
            outlined
            min-height="55"
            elevation="7"
            color="white"
            block
            class="ma-2 pa-3"
            min-width="100">
            <v-card-subtitle> {{card_name}} </v-card-subtitle>
            </v-card>
        </template>
        <v-card>
        <v-card-text>
          <v-container>
              <v-col cols="12">
                <v-flex>
                    <h2>{{card_name}}</h2>
                    <v-card flat class="ma-5">
                    <h3>Description</h3>
                    <v-flex class="ml-2">{{card_description}}</v-flex>
                    <h3>Deadline</h3>
                    <v-flex class="ml-2">{{card_term}}</v-flex>
                    </v-card>
                </v-flex>
              </v-col>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="cardDialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
        </v-card>
    </v-dialog>
</v-card>
</template>
<script>
export default {
    props: ['card_id', 'card_name', 'card_description', 'card_order', 'card_archived', 'card_term'],
    data () {
    return {
      cardDialog: false
    }
  }
}
</script>
