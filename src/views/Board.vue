<template>
    <div fluid fill-height>
    <!--  -->
    <v-app-bar
      flat
      color="rgb(0, 0, 0, 0.2)"
      class="pl-5"
      dark
      dense
    >
     <div v-if="!editing">
        <span class='text' @click="enableEditing">{{value}}</span>
      </div>
      <div v-if="editing">
        <input v-model="tempValue" class="input"/>
            <v-btn @click="disableEditing" type="button"
            class="ma-1"
            color="rgb(0, 0, 0, 0.01)"
            dark
            small>
            Cancel
          </v-btn>
          <v-btn @click="saveEdit" type="button"
            class="ma-1"
            color="rgb(0, 0, 0, 0.01)"
            dark
            small>
            Save
          </v-btn>
      </div>
      <v-spacer></v-spacer>
      <div>
        <userAddModal v-show="isUserAddModalVisible" :board_id="variableAtParent"/>
      </div>
      <v-btn @click="routeArchived()"
        class="ma-1 mr-4"
        small
        color="rgb(0, 0, 0, 0.01)"
        dark>
        Archived
      </v-btn>
    </v-app-bar>
    <v-container class="my-2">
      <v-layout row class="mt-1">
        <Container v-for="list in nonArchivedLists" :key="list.id" xs8 md3 lg3>
            <Draggable>
              <BoardList :listName="list.name" :listid="list.id" :isArchived="list.archived"/>
            </Draggable>
        </Container>
        <v-flex xs8 md3 lg3 class="mt-2">
          <listAddModal v-show="isModalVisible" :boardid="variableAtParent"/>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
<script>
import BoardList from '../components/Board/BoardList.vue'
import { getAPI } from '../api/axios-base'
import listAddModal from '../components/Board/ListAddModal.vue'
import userAddModal from '../components/Board/UserAddModal.vue'
import { Draggable, Container } from 'vue-smooth-dnd'
export default {
    components: {
      BoardList,
      listAddModal,
      userAddModal,
      Draggable,
      Container
      },
    data () {
    return {
      variableAtParent: this.$route.params.id,
      boardLists: [],
      isModalVisible: true,
      isUserAddModalVisible: true,
      value: this.$route.params.name,
        tempValue: null,
        editing: false
      }
    },
    computed: {
    nonArchivedLists: function () {
      return this.boardLists.filter(function (list) {
          return list.archived === false
        })
      }
    },
    created () {
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
    },
    methods: {
      routeArchived () {
          var archivedURL = '/board/'
          archivedURL = archivedURL.concat(this.$route.params.id, '/', this.$route.params.name, '/archived')
          this.$router.push({ path: archivedURL })
        },
        enableEditing: function () {
            this.tempValue = this.value
            this.editing = true
        },
        disableEditing: function () {
            this.tempValue = null
            this.editing = false
        },
        saveEdit: function () {
          this.value = this.tempValue
            getAPI.put('/boards/update',
              {
                id: this.variableAtParent,
                name: this.value
              },
              { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } }).then(response => {
                console.log('GetAPI successfully changed name of board')
                // window.location.reload()
              })
              .catch(err => { // refresh token expired or some other error status
                console.log(err)
              })
            this.disableEditing()
        }
    }
}
</script>
