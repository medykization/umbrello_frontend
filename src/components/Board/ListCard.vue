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
                    <editable-text v-model="cardName" :placeholder="cardName" />
                    <v-card flat class="ma-5">
                    <h3>Description</h3>
                    <editable-text v-model="description" :placeholder="description" />
                    <h3>Deadline</h3>
                            <div v-if="!editing">
                              <span class='text' @click="enableEditing">{{value}}</span>
                            </div>
                            <div v-if="editing">
                              <v-text-field
                                v-model="tempValue"
                                label="Deadline"
                                type="date"
                                name="Term"
                                id="Term"
                              ></v-text-field>
                            </div>
                    </v-card>
                </v-flex>
              </v-col>
          </v-container>
        </v-card-text>
        <v-card-actions>
           <v-btn
            color="blue darken-1"
            text
            @click="archivize"
          >
            Archive
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="saveAndClose"
          >
            Save
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="cardDialog = false;editing = false"
          >
            Close
          </v-btn>
        </v-card-actions>
        </v-card>
    </v-dialog>
</v-card>
</template>
<script>

import EditableText from '@marshal/vue-editable-text'
import { getAPI } from '../../api/axios-base'
export default {
    components: {
        EditableText
    },
    props: ['card_id', 'card_name', 'card_description', 'card_order', 'card_archived', 'card_term'],
    data () {
    return {
      cardDialog: false,
      description: this.card_description,
      cardName: this.card_name,
      value: this.card_term,
        tempValue: this.card_term,
        editing: false
    }
  },
  methods: {
        saveAndClose () {
              getAPI.put('/boards/update/card',
              {
                id: this.card_id,
                name: this.cardName,
                description: this.description,
                term: this.tempValue
              },
              { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
                console.log('GetAPI successfully changed card')
                window.location.reload()
              })
              .catch(err => { // refresh token expired or some other error status
                console.log(err)
              })
              this.disableEditing()
        },
        archivize () {
            getAPI.put('/boards/archive/card',
              {
                id: this.card_id
              },
              { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
                console.log('GetAPI successfully archived card')
                window.location.reload()
              })
              .catch(err => { // refresh token expired or some other error status
                console.log(err)
              })
        },
        enableEditing: function () {
            this.tempValue = this.value
            this.editing = true
        },
        disableEditing: function () {
            this.tempValue = this.value
            this.editing = false
        }
    }
}
</script>
