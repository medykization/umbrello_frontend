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
            elevation="1"
            color="white"
            block
            class="pa-1 ma-2"
            min-width="100">
            <v-card-subtitle> {{card_name}} </v-card-subtitle>
            </v-card>
        </template>
        <v-card>
        <v-card-text>
          <v-container>
              <v-col cols="12">
                <v-flex>
                    {{card_name}}
                    <v-card flat class="ma-5">
                    <h3>Description</h3>
                    {{card_description}}
                    <h3>Deadline</h3>
                    {{card_term}}
                    </v-card>
                </v-flex>
              </v-col>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-btn
            class="ml-1 mb-1"
            color="red darken-1"
            small
            dark
            @click="deleteCard"
          >
            Delete
          </v-btn>
           <v-btn
            color="blue darken-1"
            text
            small
            @click="unarchiveCard"
          >
            Unarchive
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            small
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

import { getAPI } from '../../api/axios-base'
export default {
    props: ['card_id', 'card_name', 'card_description', 'card_order', 'card_archived', 'card_term'],
    data () {
    return {
      cardDialog: false,
      description: this.card_description,
      cardName: this.card_name
    }
  },
  methods: {
        unarchiveCard () {
            getAPI.put('/boards/archive/card',
              {
                id: this.card_id
              },
              { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
                console.log('GetAPI successfully archived card')
                // window.location.reload()
              })
              .catch(err => { // refresh token expired or some other error status
                console.log(err)
              })
        },
        deleteCard () {
            getAPI.post('/boards/delete/card',
              {
                id: this.card_id
              },
              { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
                console.log('GetAPI successfully archived card')
                // window.location.reload()
              })
              .catch(err => { // refresh token expired or some other error status
                console.log(err)
              })
        }
    }
}
</script>
