<template>
    <v-card
        elevation="5"
        block
        min-height="100"
        min-width="100"
        max-width="300"
        class="ma-4">
        <v-card-title class="grey lighten-5">
            <h5>{{listName}}</h5>
            <v-spacer></v-spacer>
            <v-menu bottom left>
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                light
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item
                link
                @click="unArchivizeList">
                <v-list-item-title>
                        UnArchivize list
                </v-list-item-title>
              </v-list-item>
              <v-list-item
                link
                @click="deleteList">
                <v-list-item-title>
                        Delete list
                </v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-card-title>
        <div v-for="card in cards" :key="card.id">
          <ListCard :card_name="card.name"/>
        </div>
        <v-card-actions>
          <v-btn
            color="blue lighten-1"
            text
          >
          + Add new card
          </v-btn>
        </v-card-actions>
    </v-card>
</template>
<script>
import ListCard from './ListCard.vue'
import { getAPI } from '../../api/axios-base'
export default {
    components: {
      ListCard
    },
    props: ['listName', 'listid', 'isArchived'],
    data () {
      return {
        isModalVisible: true,
        variableAtParent: this.listid,
        cards: []
      }
    },
    created () {
        getAPI.post('/boards/cards', { id: this.listid }, {
          headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }) // proof that your access token is still valid; if not the
          // axios getAPI response interceptor will attempt to get a new  access token from the server. check out ../api/axios-base.js getAPI instance response interceptor
          .then(response => {
            console.log('GetAPI successfully got the cards')
            console.log(response.data)
            this.cards = response.data
            // this.$store.state.APIData = response.data // store the response data in store
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
    },
    methods: {
        unArchivizeList () {
        getAPI.put('/boards/archive/list',
          { id: this.listid },
          { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` }
        })
        .then(response => {
            console.log('GetAPI successfully added the board')
            console.log(response.data)
            window.location.reload()
            // this.$store.state.APIData = response.data // store the response data in store
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
        },
        deleteList () {
          getAPI.delete('/boards/delete/list',
            { id: this.listid },
            { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` }
          })
          .then(response => {
            console.log('GetAPI successfully delete the list')
            // window.location.reload()
            // this.$store.state.APIData = response.data // store the response data in store
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
        }
    }
}
</script>
