
<template>
  <v-container grid-list-lg>
    {{ message }}

    <v-container
      fluid
      grid-list-xl
    >

      <v-flex
        xs12
        sm3
        d-flex
      >

        <v-select
          :items="items"
          label="select type"
        />

      </v-flex>

      <v-flex
        xs12
        lg3
      />
      <v-spacer />

      <v-layout
        align-center
        justify-start
        row
        wrap
      >
        <v-flex
          v-for="room in rooms"
          :key="room.id"
          xs4
        >

          <v-card>
            <a
              class="card-link"
              href="/RoomDetail"
            >
              <v-img
                class="black--text"
                height="100px"
              >
                <v-container
                  fill-height
                  fluid
                >
                  <v-layout justify-space-around />
                </v-container>
              </v-img>

              <v-card-title>
                <div>
                  <span class="grey--text">{{ room.title }}</span><br>
                  <span />{{ room.name }}<br>
                  <span :style="{color: room.color}">status : {{ room.status }} ({{ room.startTime }}-{{ room.endTime }})</span>
                </div>
              </v-card-title>
            </a>
          </v-card>
        </v-flex>
      </v-layout>
      <!-- </v-layout> -->
    </v-container>
  </v-container>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import format from 'date-fns/format'

export default {
  data () {
    return {
      rooms: [
        // {
        //   id: '',
        //   title: '',
        //   name: '',
        //   status: 'Available',
        //   startTime: '',
        //   endTime: '',
        //   color: 'green'
        // }
      ],
      message: 'Welcome to Meetee Administrator',
      items: ['room', 'seat']

      // date: new Date().toISOString().substr(0, 10)
      // menu1: false,
      // time: null
    }
  },

  computed: {
    computedDateFormattedMomentjs () {
      return this.date ? moment(this.date).format('dddd, MMMM Do YYYY') : ''
    },
    computedDateFormattedDatefns () {
      return this.date ? format(this.date, 'dddd, MMMM Do YYYY') : ''
    }
  },

  created () {
    axios
      .post('http://18.139.5.203:9000/check/available', {
        'type': '1',
        // 'startDate': 'April 14, 2019',
        // 'startTime': '15:00:00',
        // 'endTime': '17:00:00'
        'startDate': 'May 25, 2019',
        'startTime': '21:00:00',
        'endTime': '22:00:00'
      })
      .then(response => {
        let res = response.data.availableList
        console.log(res)
        console.log(res.length)
        for (let i = 0; i < res.length; i++) {
          this.rooms.push({
            id: res[i].roomId,
            name: res[i].roomCode
          })
          console.log('at: ' + i + '\nid: ' + res[i].roomId + '\nname: ' + res[i].roomCode)
        }
      })
      .catch(e => {
        console.log(e)
      })
  }
}
</script>

<style lang="scss" scoped>
.text-red {
  color: red
}
.text-green {
  color: green
}
</style>
