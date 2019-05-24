
<template>
  <v-container grid-list-lg>
    {{ message }}

    <v-container
      fluid
      grid-list-xl>

      <v-flex
        xs12
        sm3
        d-flex>

        <v-select
          :items="items"
          label="select type"
        />

      </v-flex>

      <v-flex
        xs12
        lg3>
        <v-menu
          v-model="menu1"
          :close-on-content-click="false"
          full-width
          max-width="290"
        >

          <v-text-field
            :value="computedDateFormattedMomentjs"
            clearable
            label="select date"
            readonly
          />

          <!-- <template v-slot:activator="{ on }">
            <v-text-field
              :value="computedDateFormattedMomentjs"
              clearable
              label="select date"
              readonly
              v-on="on"
            />
          </template> -->

          <!-- <v-date-picker
            v-model="date"
            @change="menu1 = false"
          />
        </v-menu>
      </v-flex>
      </v-layout>
    </v-container>

    <v-layout
      row
      wrap>
      <v-flex
        xs11
        sm3>
        <v-menu
          ref="menu"
          v-model="menu2"
          :close-on-content-click="false"
          :nudge-right="40"
          :return-value.sync="time"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          max-width="290px"
          min-width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="time"
              label="start time"
              readonly
              v-on="on"
            />
          </template>
          <v-time-picker
            v-if="menu2"
            v-model="time"
            full-width
            @click:minute="$refs.menu.save(time)"
          />
        </v-menu>
        <v-menu
          ref="menu"
          v-model="menu3"
          :close-on-content-click="false"
          :nudge-right="40"
          :return-value.sync="time"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          max-width="290px"
          min-width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="time"
              label="end time"
              readonly
              v-on="on"
            />
          </template>
          <v-time-picker
            v-if="menu3"
            v-model="time"
            full-width
            @click:minute="$refs.menu.save(time)"
          /> -->

        </v-menu>

      </v-flex>
      <v-spacer/>

      <v-layout
        align-center
        justify-start
        row
        wrap>
        <v-flex
          v-for="room in rooms"
          :key="room.id"
          xs4
        >

          <v-card>
            <a
              class="card-link"
              href="/RoomDetail">

              <v-img
                class="black--text"
                height="100px"
              >
                <v-container
                  fill-height
                  fluid>

                  <v-layout justify-space-around/>
                </v-container>
              </v-img>

              <v-card-title>
                <div>
                  <span class="grey--text">{{ room.title }}</span><br>
                  <span/>{{ room.name }}<br>
                  <span :style="{color: room.color}">status : {{ room.status }} ({{ room.startTime }}-{{ room.endTime }})</span>
                </div>
              </v-card-title>

          </a></v-card>
        </v-flex>
      </v-layout>
    </v-layout></v-container>
</v-container></template>

<script>
import axios from 'axios'
import moment from 'moment'
import format from 'date-fns/format'

export default {
  data () {
    return {
      rooms: [
        {
          id: '',
          title: '',
          name: '',
          status: 'Available',
          startTime: '',
          endTime: '',
          color: 'green'
        }
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
    axios.get(`http://18.139.5.203:9000/rooms`)
      .then(response => {
        let res = response.data
        console.log(res)
        console.log(res.length)
        for (let i = 0; i < res.length; i++) {
          this.rooms[0].id = res[i].id
          this.rooms[0].name = res[i].name
          // console.log('id at' + i + ' : ' + this.id + ', `name` ' + name)
        }
      })
      .catch(e => {
        console.log(e)
      })
  }
}

</script>

<style lang="scss" scoped>

.text-red{
  color: red
}
.text-green{
  color: green
}
</style>
