<template>
    <div fluid fill-height>
    <!-- <header>{{ $route.params.name }}</header> -->
    <v-container class="ma-2">
      <v-layout row wrap class="mt-1">
        <v-flex xs8 md4 lg4>
          <v-card

            elevation="5"
            block
            min-height="100"
            min-width="250"
            max-width="250"
            class="ma-2"
            color="rgb(255, 255, 255, 0.4)">
            <v-card-title><h6>ARCHIVED CARDS</h6></v-card-title>
            <v-container v-for="card in archivedCards" :key="card.id">
              <ListCard :card_name="card.name" :card_id="card.id" :card_description="card.description" :card_term="card.term" :archivize_button_text="'Unarchive'"/>
            </v-container>
            </v-card>
        </v-flex>
        <v-flex v-for="list in archivedLists" :key="list.id" xs8 md4 lg3>
            <div >
              <BoardList :listName="list.name" :listid="list.id" :isArchived="list.archived"/>
            </div>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
<script>
import BoardList from '../components/Board/ArchivedList.vue'
import ListCard from '../components/Board/ListCard.vue'
import { getAPI } from '../api/axios-base'
export default {
    components: { BoardList, ListCard },
    data () {
    return {
      variableAtParent: this.$route.params.id,
      boardLists: [],
      isModalVisible: true,
      archivedCards: []
      }
    },
    computed: {
    archivedLists: function () {
      return this.boardLists.filter(function (list) {
        return list.archived === true
      })
    }
    },
    created () {
      // GET ARCHIVED LISTS
        getAPI.post('/boards/lists', { id: this.$route.params.id }, { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }) // proof that your access token is still valid; if not the
          // axios getAPI response interceptor will attempt to get a new  access token from the server. check out ../api/axios-base.js getAPI instance response interceptor
            .then(response => {
              console.log('GetAPI successfully got the lists')
              // console.log(response.data)
              this.boardLists = response.data
              // this.$store.state.APIData = response.data // store the response data in store
            })
            .catch(err => { // refresh token expired or some other error status
              console.log(err)
            })
      // GET ARCHIVED CARDS
        getAPI.post('/boards/archived/cards', { id: this.$route.params.id }, { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }) // proof that your access token is still valid; if not the
          // axios getAPI response interceptor will attempt to get a new  access token from the server. check out ../api/axios-base.js getAPI instance response interceptor
            .then(response => {
              console.log('GetAPI successfully got the cards')
              // console.log(response.data)
              this.archivedCards = response.data
              // this.$store.state.APIData = response.data // store the response data in store
            })
            .catch(err => { // refresh token expired or some other error status
              console.log(err)
            })
      }
}
</script>
