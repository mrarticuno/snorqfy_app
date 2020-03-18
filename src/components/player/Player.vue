<template>
  <div >
    <div class="container items-center">
      <!-- MOBILE -->
      <div class="mobile-only row">
        <div class="col-12 song-name">Combatchy</div>
        <div class="col-12 song-artist">Anitta</div>
      </div>
      <div class="mobile-only row">
        <div class="col-12 text-center rating">
          <q-icon class="icon" name="thumb_up" />
          <q-icon class="icon" name="thumb_down" />
        </div>
      </div>
      <!-- END MOBILE -->
      <!-- DESKTOP -->
      <div class="mobile-hide">
        <div class="song-name">Combatchy</div>
        <div class="song-artist">Anitta</div>
      </div>
      <div class="song-vote mobile-hide">
        <a class="vote-link vote-up" href="#"><q-icon class="icon" name="thumb_up" /></a>
        <a class="vote-link vote-down" href="#"><q-icon class="icon" name="thumb_down" /></a>
      </div>
      <!-- END DESKTOP -->
      <div class="row absolute-bottom control">
        <div class="col-12">
          <div class="row">
            <div class="col-2 time-spend">1:32</div>
            <div class="col-8"><q-slider v-model="track" :min="0" :max="100" color="purple"/></div>
            <div class="col-2 time-left">2:46</div>
          </div>
        </div>
        <div class="col-12 control">
          <div class="row">
            <div class="col-2 shuffle-class self-center">
              <a class="vote-link" href="#"><q-icon name="mdi-shuffle-variant" /></a>
            </div>
            <div class="col-2 backward-class self-center">
              <a class="vote-link" href="#"><q-icon class="icon" name="mdi-step-backward" /></a>
            </div>
            <div class="col-4 play-class self-center">
              <a class="vote-link" href="#"><q-icon name="mdi-play-circle-outline" /></a>
            </div>
            <div class="col-2 forward-class self-center">
              <a class="vote-link" href="#"><q-icon class="icon" name="mdi-step-forward" /></a>
            </div>
            <div class="col-2 repeat-class self-center">
              <a class="vote-link" href="#"><q-icon class="icon" name="mdi-repeat" /></a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Player',
  data () {
    return {
      image: 'https://i.ytimg.com/vi/-YTeFMxUOAU/maxresdefault.jpg',
      track: 10
    }
  },
  mounted () {
    const { RandomPicture } = require('random-picture')

    RandomPicture().then(res => {
      console.log(res)
      debugger
    })
  }
}
</script>

<style lang="sass" scoped>
@media (min-width: $breakpoint-xs-min)
  .shuffle-class
    font-size: 20pt
    text-align: center
  .repeat-class
    font-size: 40pt
    text-align: center
  .forward-class
    font-size: 40pt
    text-align: center
  .backward-class
    font-size: 40pt
    text-align: center
  .play-class
    font-size: 40pt
    text-align: center
  .song-name
    text-align: center
  .song-artist
    text-align: center
  .song-vote
    text-align: center
  .time-spend
    text-align: center
    padding: 7pt
  .time-left
    text-align: center
    padding: 5pt
  .vote-link
    text-decoration: none
    color: white
  .vote-up
    text-decoration: none
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 3px
    -webkit-text-stroke-color: green
  .vote-down
    text-decoration: none
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 3px
    -webkit-text-stroke-color: red

@media (max-width: $breakpoint-xs-max)
  .icon
    font-size: 25pt

  .song-name
    font-size: 40pt

  .song-artist
    font-size: 25pt

  .icon
    margin: 5pt

@media (min-width: $breakpoint-sm-min)
  .song-details div
    margin: 10pt

  .song-vote .icon
    margin: 5pt

  .icon
    font-size: 25pt

  .song-name
    font-size: 35pt

  .song-artist
    font-size: 15pt

</style>
