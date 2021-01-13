<script>

  import { getAPI } from '../../api/axios-base'
  export default {
    props: ['listid'],
    data: () => ({
      dialog: false,
      name: '',
      description: '',
      term: ''
    }),
    methods: {
      addCard () {
      getAPI.post('/boards/add/card',
        {
          id: this.listid,
          name: this.name,
          description: this.description,
          term: this.term
        },
        { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
          console.log('GetAPI successfully added the board')
          window.location.reload()
          // this.$store.state.APIData = response.data // store the response data in store
        })
        .catch(err => { // refresh token expired or some other error status
          console.log(err)
        })
      }
    }
  }
</script>
<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-list-item link
          color="primary"
          v-bind="attrs"
          v-on="on"
        >
        Add new card
        </v-list-item>
      </template>
      <v-card>
          <v-card-title>
          <span class="headline">New card</span>
        </v-card-title>
        <v-card-text>
          <v-container>
              <v-col cols="12">
                 <v-text-field
                  label="New name of card"
                  type="text"
                  name="Name"
                  id="Name"
                  v-model="name"
                  required
                ></v-text-field>
                <v-text-field
                  label="Description"
                  type="text"
                  name="Description"
                  id="Description"
                  v-model="description"
                ></v-text-field>
                <v-text-field
                  label="Deadline"
                  type="date"
                  name="Term"
                  id="Term"
                  v-model="term"
                ></v-text-field>
              </v-col>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            type="button"
            @click="addCard"
          >
            Update
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
