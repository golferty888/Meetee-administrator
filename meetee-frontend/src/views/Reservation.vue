<template>
  <v-container grid-list-lg>
    {{ message }}
    <v-layout
      align-center
      justify-start
      row
      wrap>
      <v-flex
        v-for="room in rooms"
        :key="room.id"
        xs6>
        <material-stats-card
          :color="room.status"
          :title="room.title"
          :value="room.name"
          :sub-text="room.until"
          icon="mdi-store"
          sub-icon="mdi-calendar"
        />
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

const rooms = [
  {
    id: 1,
    title: 'Room',
    name: 'Small-MeetingRoom-01',
    status: 'green',
    until: 'Available'
  },
  {
    id: 2,
    title: 'Room',
    name: 'Small-MeetingRoom-02',
    status: 'red',
    until: '6 p.m.'
  },
  {
    id: 3,
    title: 'Room',
    name: 'Large-MeetingRoom-01',
    status: 'red',
    until: '6 p.m.'
  },
  {
    id: 4,
    title: 'Room',
    name: 'Large-MeetingRoom-02',
    status: 'green',
    until: 'Available'
  },
  {
    id: 5,
    title: 'Seat',
    name: 'Seat-01',
    status: 'green',
    until: 'Available'
  }
]
export default {
  data () {
    return {
      message: 'Welcome to Meetee Administrator',
      rooms
    }
  },
  created () {
    axios.get(`http://localhost:9500/type/room`)
      .then(response => {
        console.log(response.data)
      })
      .catch(e => {
        this.errors.push(e)
      })
  }
}
</script>

<style lang="scss" scoped>

</style>
