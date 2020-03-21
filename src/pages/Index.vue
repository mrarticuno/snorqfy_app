<template>
  <q-page class="flex flex-center background" :style="{ backgroundImage: `url('${bgImage}')`}">
    <player></player>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      bgImage: ''
    }
  },
  mounted () {
    const { RandomPicture } = require('random-picture')
    RandomPicture().then(res => {
      this.bgImage = res.url
    })
  },
  components: {
    Player: () => import('components/player/Player')
  }
}
</script>

<style>
  .background {
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
  }
</style>
