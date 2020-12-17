<template>
    <div fluid fill-height>
    <!-- <header>{{ $route.params.name }}</header> -->
    <v-container class="my-2">
      <v-layout row class="mt-1">
        <v-flex v-for="list in boardLists" :key="list.id" xs8 md4 lg2>
            <div v-if="list.archived===false">
              <BoardList :listName="list.name" :listid="list.id" :isArchived="list.archived"/>
            </div>
        </v-flex>
        <v-flex xs8 md4 lg2 class="mt-2">
          <v-card
              outlined
              min-height="55"
              elevation="7"
              color="white"
              block
              class="ma-2 pa-3"
              min-width="100">
            <v-card-actions>
              <!-- <v-btn color="blue" text> + Add new list </v-btn> -->
              <modal v-show="isModalVisible"/>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
<script>
import BoardList from '../components/BoardList.vue'
import { getAPI } from '../api/axios-base'
export default {
    components: { BoardList },
    data () {
    return {
      boardLists: [],
      isModalVisible: false
    }
  },
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
    }
}
</script>
