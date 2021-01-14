<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn link
          v-bind="attrs"
          v-on="on"
          class="ma-1 mr-4"
          small
          color="rgb(0, 0, 0, 0.01)"
          dark
        >INVITE</v-btn>
      </template>
      <v-card>
          <v-card-title>
          <span class="headline">Invite to board</span>
        </v-card-title>
        <v-card-text>
          <v-container>
              <v-col cols="12">
                <v-text-field
                  label="Username"
                  type="text"
                  name="Username"
                  id="Username"
                  v-model="username"
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
            @click="addUser"
          >
            Add
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import { getAPI } from '../../api/axios-base'
  export default {
    props: ['board_id'],
    data: () => ({
      dialog: false,
      username: ''
    }),
    methods: {
      addUser () {
        getAPI.post('/boards/add/user',
        {
          id: this.board_id,
          name: this.username
        },
        { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } })
      .then(response => {
          console.log('GetAPI add user')
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
