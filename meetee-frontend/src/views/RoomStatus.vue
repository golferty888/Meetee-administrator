
<template>
  <v-container grid-list-lg>
    <!-- <v-layout
      row
      wrap>
      <v-flex
        xs12
        sm6
        md4>
        <v-dialog
          ref="dateDialog"
          v-model="dateModal"
          :return-value.sync="date"
          persistent
          lazy
          full-width
          width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="date"
              label="Choose Date"
              readonly
              v-on="on"
            />
          </template>
          <v-date-picker
            v-model="date"
            dark
            scrollable>
            <v-spacer/>
            <v-btn
              flat
              color="primary"
              @click="dateModal = false">Cancel</v-btn>
            <v-btn
              flat
              color="primary"
              @click="$refs.dateDialog.save(date)">OK</v-btn>
          </v-date-picker>
        </v-dialog>
      </v-flex>
      <v-spacer/>

      <v-spacer/>
    </v-layout>
    <v-layout
      row
      wrap>
      <v-flex
        xs11
        sm5>
        <v-dialog
          ref="timeStartDialog"
          v-model="timeStartModal"
          :return-value.sync="startTime"
          persistent
          lazy
          full-width
          width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="startTime"
              label="Choose Time Start"
              readonly
              v-on="on"
            />
          </template>
          <v-time-picker
            v-if="timeStartModal"
            v-model="startTime"
            full-width
          >
            <v-spacer/>
            <v-btn
              flat
              color="primary"
              @click="timeStartModal = false">Cancel</v-btn>
            <v-btn
              flat
              color="primary"
              @click="$refs.timeStartDialog.save(time)">OK</v-btn>
          </v-time-picker>
        </v-dialog>
      </v-flex>
    </v-layout> -->
    <v-tabs
      v-model="tab"
      height="60px"
      color="black-grey"
      align-with-title
      dark
    >
      <v-tabs-slider color="white"/>
      <v-tab
        href="#tab-1"
        class="tabLabel">
        Room
      </v-tab>
      <v-tab
        href="#tab-2"
        class="tabLabel">
        Seat
      </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item
        :value="'tab-1'"
      >
        <v-container
          fluid
          grid-list-xl
        >
          <v-flex
            xs12
            lg3
          />
          <v-layout
            align-center
            justify-start
            row
            wrap
          >
            <v-flex
              v-for="room in facility[1]"
              :key="room.id"
              xs4
            >
              <material-stats-card
                :color="room.status"
                :title="'ID: ' + room.id"
                :value="room.name"
                :sub-text="room.start_time + ' - ' + room.end_time"
                icon="mdi-store"
                sub-icon="mdi-calendar"
              />
            </v-flex>
          </v-layout>
        </v-container>
      </v-tab-item>
      <v-tab-item
        :value="'tab-2'"
      >
        <v-container
          fluid
          grid-list-xl
        >
          <v-flex
            xs12
            lg3
          />
          <v-layout
            align-center
            justify-start
            row
            wrap
          >
            <v-flex
              v-for="room in facility[2]"
              :key="room.id"
              xs3
            >
              <material-stats-card
                :color="room.status"
                :title="'ID: ' + room.id"
                :value="room.name"
                :sub-text="room.start_time + ' - ' + room.end_time"
                icon="mdi-seat-recline-normal"
                sub-icon="mdi-calendar"
              />
            </v-flex>
          </v-layout>
        </v-container>
      </v-tab-item>
    </v-tabs-items>
  </v-container>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
// import format from 'date-fns/format'

export default {
  data () {
    return {
      tab: null,
      facility: [
        [], [], []
      ],
      // rooms: [
      //   // {
      //   //   id: '',
      //   //   title: '',
      //   //   name: '',
      //   //   status: 'Available',
      //   //   startTime: '',
      //   //   endTime: '',
      //   //   color: 'green'
      //   // }
      // ],
      message: 'Welcome to Meetee Administrator',
      items: ['room', 'seat'],

      date: new Date().toISOString().substr(0, 10),
      dateModal: false,

      startTime: null,
      timeStartModal: false
    }
  },

  // computed: {
  //   computedDateFormattedMomentjs () {
  //     return this.date ? moment(this.date).format('dddd, MMMM Do YYYY') : ''
  //   },
  //   computedDateFormattedDatefns () {
  //     return this.date ? format(this.date, 'dddd, MMMM Do YYYY') : ''
  //   }
  // },

  created () {
    // console.log(moment().format('MMMM DD, YYYY'))
    var startDate = moment().format('MMMM') + ' ' + moment().format('DD') + ', ' + moment().format('YYYY')
    var startTime = moment().format('hh') + ':' + moment().format('mm') + ':' + moment().format('ss')
    var addHour = moment().add(1, 'HH')
    var endTime = moment(addHour).format('HH:mm:ss')
    // var mockDate = {
    //   'type': '1',
    //   'startDate': startDate,
    //   'startTime': startTime,
    //   'endTime': endTime
    // }
    axios
      .post('http://18.139.12.132:9000/check/unavailable', {
        // 'type': '1',
        // 'startDate': startDate,
        // // 'startDate': 'April 14, 2019',
        // 'startTime': startTime,
        // 'endTime': endTime
        'type': '1',
        'startDate': startDate,
        'startTime': startTime,
        'endTime': endTime
      }
      )
      .then(response => {
        let res = response.data.unAvailableList
        console.log('room')
        console.log(res)
        // console.log(this.facility[1])
        for (let i = 0; i < res.length; i++) {
          // this.rooms.push({
          //   id: res[i].roomId,
          //   name: res[i].roomCode
          // })
          this.facility[1].push({
            id: res[i].room_id,
            name: res[i].room_code,
            start_time: res[i].start_time,
            end_time: res[i].end_time,
            status: 'red'
          })
          // console.log('at: ' + i + '\nid: ' + res[i].roomId + '\nname: ' + res[i].roomCode)
        }
      })
      .catch(e => {
        console.log(e)
      })

    axios
      .post('http://18.139.12.132:9000/check/unavailable', {
        'type': '2',
        'startDate': startDate,
        'startTime': startTime,
        'endTime': endTime
      })
      .then(response => {
        let res = response.data.unAvailableList
        console.log('seat')
        console.log(res)
        // console.log(this.facility[0])
        for (let i = 0; i < res.length; i++) {
          // this.rooms.push({
          //   id: res[i].roomId,
          //   name: res[i].roomCode
          // })
          this.facility[2].push({
            id: res[i].room_id,
            name: res[i].room_code,
            start_time: res[i].start_time,
            end_time: res[i].end_time,
            status: 'red'
          })
          // console.log('at: ' + i + '\nid: ' + res[i].roomId + '\nname: ' + res[i].roomCode)
        }
        // console.log(this.facility[2])
      })
      .catch(e => {
        console.log(e)
      })
  }
}
</script>

<style lang="scss" scoped>
.tabLabel {
  font-size: 24px !important
}

</style>
