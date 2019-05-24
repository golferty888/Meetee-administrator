<template>
  <div class="RoomDetails">
    <h1>Details of {{ roomId }}</h1>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: function () {
    return {
      roomId: this.$route.params.id

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
