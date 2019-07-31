<template>
  <div id="app">

    <div class="wrapper">
      <div class="desktop">
        <div class="side-left">
          <div class="header-block">
            <h1 class="logo">Music Box</h1>
            <a href="javascript:;" class="header-btn icon-home">Home</a>
            <a href="javascript:;" class="header-btn icon-discover">Discover</a>
            <a href="javascript:;" class="header-btn icon-radio">Radio</a>
          </div>
          <div class="library-block">
            <span class="mark">Your Music Library</span>
            <a href="javascript:;" class="lib-btn">Recently played</a>
            <a href="javascript:;" class="lib-btn">Liked music</a>
            <a href="javascript:;" class="lib-btn">Albums</a>
            <a href="javascript:;" class="lib-btn">Singer</a>
          </div>
          <figure class="album-block">
            
          </figure>
        </div>
        <div class="side-center">
          <div class="banner-block"></div>
          <div class="music-block">
            <h2>Top Song</h2>
          </div>
        </div>
        <div class="side-right">
          <div class="member"></div>
          <span class="mark">Your Music List</span>
          <ul class="songlist-list">
            <li></li>
          </ul>
        </div>
      </div>
      <div class="contral">
        <youtube :video-id="player.id" width="270" height="152" ref="youtube" :player-vars="youtubeset" @buffering="playing"></youtube>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import VueYoutube from 'vue-youtube';
Vue.use(VueYoutube);

export default {
  data() {
    return {
      player: {
        id: '',
        pic: '',
        title: '',
        artist: '',
        album: '',
      },
      youtubeset: {
        id: '',
        option: {
          rel: 0,
          fs: 1,
          iv_load_policy: 3,
          modestbranding: 1,
          playsinline: 1
        }
      },
      music: [
        {
          id: 'w1oM3kQpXRo',
          pic: '',
          title: 'Everything Has Changed',
          artist: 'Taylor Swift',
          album: 'Red',
        },
      ]
    }
  },
  name: 'app',
  components: {
  },
  mounted() {
    this.replaceAudio(0);
    this.audioPlay();
  },
  methods: {
    audioPlay() {
      this.$nextTick(function(){
        this.videoPlayer.playVideo();
      });
    },
    audioPause() {
      this.videoPlayer.pauseVideo();
    },
    playing() {
      // this.videoPlayer.getCurrentTime();
      console.log('aaa')
    },
    replaceAudio(index) {
      this.player.id = this.music[index].id;
      this.player.pic = this.music[index].pic;
      this.player.title = this.music[index].title;
      this.player.artist = this.music[index].artist;
      this.player.album = this.music[index].album;

      this.youtubeset.id = this.player.id;
    }
  },
  computed: {
    videoPlayer() {
      return this.$refs.youtube.player
    }
  },
}
</script>

<style lang="scss" scoped>
  @import './assets/scss/_utils.scss';
  @import './assets/scss/main.scss';

</style>
