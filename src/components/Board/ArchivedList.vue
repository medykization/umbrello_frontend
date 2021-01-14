<template>
    <v-card
        elevation="5"
        block
        min-height="100"
        min-width="100"
        max-width="300"
        class="ma-4">
        <v-card-title class="grey lighten-5">
            <h6>{{listName}}</h6>
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
                        Unarchive list
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
          <ArchivedCard :card_id="card.id" :card_name="card.name" :card_description="card.description" :card_term="card.term"/>
        </div>
    </v-card>
</template>
<script>
import ArchivedCard from './ArchivedCard.vue'
import { getAPI } from '../../api/axios-base'
export default {
    components: {
      ArchivedCard
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
        getAPI.post('/boards/cards', { id: this.listid }, { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } })
          .then(response => {
            this.cards = response.data
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
    },
    methods: {
        unArchivizeList () {
        getAPI.put('/boards/archive/list',
          { id: this.listid },
          { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
            window.location.reload()
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
        },
        deleteList () {
          getAPI.post('/boards/delete/list',
            { id: this.listid },
            { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } })
          .then(response => {
            window.location.reload()
          })
          .catch(err => { // refresh token expired or some other error status
            console.log(err)
          })
        }
    }
}
</script>
