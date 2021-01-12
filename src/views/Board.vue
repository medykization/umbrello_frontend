<template>
    <div fluid fill-height>
    <!--  -->
    <v-app-bar
      flat
      color="rgb(0, 0, 0, 0.2)"
      class="pl-5"
      dark
    >
      <v-toolbar-title>{{ $route.params.name }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        class="ma-1"
        color="rgb(0, 0, 0, 0.01)"
        dark
      >
        SMTH
      </v-btn>
      <v-btn
        class="ma-1"
        color="rgb(0, 0, 0, 0.01)"
        dark
      >
        SMTH
      </v-btn>
      <v-btn @click="routeArchived()" type="button"
        class="ma-1"
        color="rgb(0, 0, 0, 0.01)"
        dark>
        Archived
      </v-btn>
    </v-app-bar>
    <v-container class="my-2">
      <v-layout row class="mt-1">
        <v-flex v-for="list in nonArchivedLists" :key="list.id" xs8 md4 lg3>
            <div >
              <BoardList :listName="list.name" :listid="list.id" :isArchived="list.archived"/>
            </div>
        </v-flex>
        <v-flex xs8 md4 lg3 class="ma-2">
          <v-card
              min-height="120"
              elevation="7"
              color="white"
              block
              class="ma-2 pa-3"
              min-width="100">
            <v-card-actions>
                <v-card-actions>
                  <modal v-show="isModalVisible" :boardid="variableAtParent"/>
                </v-card-actions>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
<script>
import BoardList from '../components/Board/BoardList.vue'
import { getAPI } from '../api/axios-base'
import modal from '../components/Board/ListAddModal.vue'
export default {
    components: { BoardList,
    modal },
    data () {
    return {
      variableAtParent: this.$route.params.id,
      boardLists: [],
      isModalVisible: true
      }
    },
    computed: {
    nonArchivedLists: function () {
      return this.boardLists.filter(function (list) {
        return list.archived === false
      })
    } },
    created () {
        getAPI.post('/boards/lists', { id: this.$route.params.id }, {
          headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }) // proof that your access token is still valid; if not the
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
    },
    methods: {
      routeArchived () {
          var archivedURL = '/board/'
          archivedURL = archivedURL.concat(this.$route.params.id, '/', this.$route.params.name, '/archived')
          this.$router.push({ path: archivedURL })
        }
      }
}
</script>
