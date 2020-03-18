<template>
  <q-page class="flex flex-center">
    <player></player>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
    }
  },
  components: {
    Player: () => import('components/player/Player')
  }
}
</script>
