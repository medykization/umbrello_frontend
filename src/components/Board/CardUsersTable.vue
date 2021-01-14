<template>
  <v-simple-table dense>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">
            Users
          </th>
          <th class="text-left">
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in users"
          :key="item.name"
        >
          <td>{{ item.name }}</td>
          <td>
              <v-simple-checkbox
              v-model="item.checked"
              v-bind:value="item.checked"
              :ripple="false"
              @click="changeValue(item.name, item.checked)">
              </v-simple-checkbox>
          </td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
import { getAPI } from '../../api/axios-base'
  export default {
      props: ['card_id'],
    data () {
      return {
        users: [
          {
            name: 'admin',
            checked: false
          },
          {
            name: 'testowy1',
            checked: false
          },
          {
            name: 'testowy2',
            checked: false
          }
        ]
      }
    },
    methods: {
        changeValue (user, value) {
            console.log('przed')
            if (value === true) {
                console.log('po')
                this.addUserToCard(user)
            }
        },
        addUserToCard (user) {
            getAPI.post('/boards/add/user/card',
            {
            id: this.card_id,
            name: user
            },
            { headers: { Authorization: `Bearer ${this.$store.state.accessToken}` } })
        .then(response => {
            console.log('GetAPI add user to card')
            // this.$store.state.APIData = response.data // store the response data in store
            })
            .catch(err => { // refresh token expired or some other error status
            console.log(err)
            })
        }
    }
  }
//   boards/add/user/card           id, name
</script>
