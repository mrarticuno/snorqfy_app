<template>
  <div>
    <div class="container items-center">
      <audio id="audiotag" @timeupdate="currentTime = $event.target.currentTime">
        <source :src="getUrlSong" type="audio/mpeg">
      </audio>
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
        <div class="song-name">{{currentSong.title}}</div>
      </div>
      <div class="song-vote mobile-hide">
        <a class="vote-link vote-up" href="#"><q-icon class="icon" name="thumb_up" /></a>
        <a class="vote-link vote-down" href="#"><q-icon class="icon" name="thumb_down" /></a>
      </div>
      <!-- END DESKTOP -->
      <div class="row absolute-bottom control">
        <div class="col-12">
          <div class="row">
            <div class="col-2 time-spend">{{fancyTimeFormat(currentTime)}}</div>
            <div class="col-8"><q-slider @input="changeTime" v-model="currentTime" :min="0" :max="player.duration ? player.duration : 0" color="purple"/></div>
            <div class="col-2 time-left">{{fancyTimeFormat(player.duration)}}</div>
          </div>
        </div>
        <div class="col-12 control">
          <div class="row">
            <div class="col-2 shuffle-class self-center">
              <a class="vote-link" href="#" @click="changeShuffle()"><q-icon :name="shuffleIcon" /></a>
            </div>
            <div class="col-2 backward-class self-center">
              <a class="vote-link" href="#" @click="playPreviousSong()"><q-icon class="icon" name="mdi-step-backward" /></a>
            </div>
            <div class="col-4 play-class self-center">
              <a class="vote-link" href="#" @click="playPauseSong()"><q-icon :name="playPauseIcon" /></a>
            </div>
            <div class="col-2 forward-class self-center">
              <a class="vote-link" href="#" @click="playNextSong()"><q-icon class="icon" name="mdi-step-forward" /></a>
            </div>
            <div class="col-2 repeat-class self-center">
              <a class="vote-link" href="#" @click="changeRepeat()"><q-icon class="icon" :name="repeatIcon" /></a>
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
      track: 10,
      currentTime: 0,
      currentTrackIndex: 0,
      songs: [],
      trackList: [],
      currentSong: {},
      repeatType: 0,
      shuffleType: 0,
      playPauseIcon: 'mdi-play-circle-outline',
      player: {}
    }
  },
  watch: {
    currentTime: function (newValue, oldValue) {
      if (newValue + 1 >= this.player.duration) {
        if (this.repeatType === 2) {
          this.currentTime = 0
          this.player.play()
        } else {
          this.playNextSong()
        }
      }
    }
  },
  methods: {
    playPauseSong () {
      if (this.player.paused) {
        this.playPauseIcon = 'mdi-pause-circle-outline'
        this.player.play()
      } else {
        this.playPauseIcon = 'mdi-play-circle-outline'
        this.player.pause()
      }
    },
    changeTime (value) {
      this.player.currentTime = value
    },
    changeRepeat () {
      this.repeatType++
      if (this.repeatType === 3) {
        this.repeatType = 0
      }
    },
    changeShuffle () {
      this.shuffleType++
      if (this.shuffleType === 2) {
        this.shuffleType = 0
      }
    },
    fancyTimeFormat (time) {
      let hrs = ~~(time / 3600) // ~~ is a shorthand for Math.floor
      let mins = ~~((time % 3600) / 60)
      let secs = ~~time % 60
      let ret = ''
      if (hrs > 0) {
        ret += '' + hrs + ':' + (mins < 10 ? '0' : '')
      }
      ret += '' + mins + ':' + (secs < 10 ? '0' : '')
      ret += '' + secs
      return ret
    },
    playNextSong () {
      if (this.trackList[this.currentTrackIndex + 1]) {
        this.currentTrackIndex++
        this.currentSong = this.trackList[this.currentTrackIndex]
        this.player.load()
        this.player.play()
      } else if (this.repeatType === 0) {
        this.currentTrackIndex = 0
        this.currentSong = this.trackList[this.currentTrackIndex]
        this.player.load()
        this.player.play()
      }
    },
    playPreviousSong () {
      if (this.trackList[this.currentTrackIndex - 1]) {
        this.currentTrackIndex--
        this.currentSong = this.trackList[this.currentTrackIndex]
        this.player.load()
        this.player.play()
      } else if (this.repeatType === 0) {
        this.currentTrackIndex = this.trackList[this.trackList.length - 1]
        this.currentSong = this.trackList[this.currentTrackIndex]
        this.player.load()
        this.player.play()
      }
    },
    shuffle (array) {
      let currentIndex = array.length, temporaryValue, randomIndex
      while (currentIndex !== 0) {
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex -= 1
        temporaryValue = array[currentIndex]
        array[currentIndex] = array[randomIndex]
        array[randomIndex] = temporaryValue
      }
      return array
    },
    enqueueSongs () {
      if (this.shuffleType === 0) {
        this.trackList = this.shuffle(this.songs)
      } else {
        this.trackList = this.songs
      }
    },
    getAllSongs () {
      this.$axios.get('http://localhost:3000/playlist/').then(result => {
        this.songs = result.data.song
        this.enqueueSongs()
        this.currentSong = this.trackList[0]
        this.player.load()
      })
    }
  },
  computed: {
    getUrlSong () {
      return `http://localhost:3000/listen/${this.currentSong.code}`
    },
    repeatIcon () {
      let icon = 'mdi-repeat'
      switch (this.repeatType) {
        case 0:
          icon = 'mdi-repeat'
          break
        case 1:
          icon = 'mdi-repeat-off'
          break
        case 2:
          icon = 'mdi-repeat-once'
      }
      return icon
    },
    shuffleIcon () {
      let icon = 'mdi-shuffle-variant'
      switch (this.shuffleType) {
        case 0:
          icon = 'mdi-shuffle-variant'
          break
        case 1:
          icon = 'mdi-shuffle-disabled'
          break
      }
      return icon
    }
  },
  mounted () {
    this.player = document.getElementById('audiotag')
    this.getAllSongs()
  }
}
</script>

<style lang="sass" scoped>
@media (min-width: $breakpoint-xs-min)
  .shuffle-class
    font-size: 20pt
    text-align: center
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 2px
    -webkit-text-stroke-color: black
  .repeat-class
    font-size: 40pt
    text-align: center
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 2px
    -webkit-text-stroke-color: black
  .forward-class
    font-size: 40pt
    text-align: center
    -webkit-text-fill-color: black
    -webkit-text-stroke-width: 2px
    -webkit-text-stroke-color: black
  .backward-class
    font-size: 40pt
    text-align: center
    -webkit-text-fill-color: black
    -webkit-text-stroke-width: 2px
    -webkit-text-stroke-color: black
  .play-class
    font-size: 40pt
    text-align: center
  .song-name
    text-align: center
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 1px
    -webkit-text-stroke-color: black
  .song-artist
    text-align: center
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 1px
    -webkit-text-stroke-color: yellow
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
    -webkit-text-stroke-width: 2px
    -webkit-text-stroke-color: green
  .vote-down
    text-decoration: none
    -webkit-text-fill-color: white
    -webkit-text-stroke-width: 2px
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
