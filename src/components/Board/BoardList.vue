<template>
    <v-card
        elevation="5"
        block
        min-height="100"
        min-width="250"
        max-width="250"
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
              <v-list-item link>
                <v-list-item-title>
                        <cardAddModal v-show="isCardAddModalVisible" :listid="variableAtParent"/>
                </v-list-item-title>
              </v-list-item>
              <v-list-item link>
                <v-list-item-title>
                    <v-list-item-action>
                        <listEditModal v-show="isListEditModalVisible" :listid="variableAtParent"/>
                    </v-list-item-action>
                </v-list-item-title>
              </v-list-item>
              <v-list-item
                link
                @click="archivizeList">
                <v-list-item-title>
                        Archivize list
                </v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-card-title>
        <container>
        <Draggable v-for="card in cards" :key="card.id">
          <ListCard :card_name="card.name"/>
        </Draggable>
        </container>
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
import listEditModal from './ListEditModal.vue'
import cardAddModal from './CardAddModal.vue'
import { getAPI } from '../../api/axios-base'
import { Draggable, Container } from 'vue-smooth-dnd'
export default {
    components: {
      listEditModal,
      cardAddModal,
      ListCard,
      Container,
      Draggable
    },
    props: ['listName', 'listid', 'isArchived'],
    data () {
      return {
        isCardAddModalVisible: true,
        isListEditModalVisible: true,
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
        archivizeList () {
        getAPI.put('/boards/archive/list',
          { id: this.listid },
          { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` }
        })
        .then(response => {
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
