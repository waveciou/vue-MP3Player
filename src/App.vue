<template>
  <div id="app">

    <div class="wrapper">
      <div class="desktop">
        <div class="side-left">
          <div class="header">
            <h1 class="logo">Music Box</h1>
            <a href="javascript:;" class="header-btn icon-home">Home</a>
            <a href="javascript:;" class="header-btn icon-discover">Discover</a>
            <a href="javascript:;" class="header-btn icon-radio">Radio</a>
          </div>
          <div class="library">
            <span class="mark">Your Music Library</span>
            <a href="javascript:;" class="lib-btn">Recently played</a>
            <a href="javascript:;" class="lib-btn">Liked music</a>
            <a href="javascript:;" class="lib-btn">Albums</a>
            <a href="javascript:;" class="lib-btn">Singer</a>
          </div>
          <figure class="album"></figure>
        </div>
        <div class="side-center"></div>
        <div class="side-right">
          <div class="member"></div>
          <span class="mark">Your Music List</span>
          <ul class="music-list">
            <li></li>
          </ul>
        </div>
      </div>
      <div class="contral">

      </div>
    </div>

    <audio>
      <source :src="player.src" >
    </audio>
    <a href="javascript:;" @click="audioPlay()">安安</a>




  </div>
</template>

<script>

export default {
  data() {
    return {
      player: {
        dom: null,
        src: '',
        pic: '',
        title: '',
        artist: '',
        album: '',
      },
      music: [
        {
          src: require('../public/music/Central_Park.mp3'),
          pic: '',
          title: 'Central Park',
          artist: 'Quincas Moreira',
          album: 'You Tube 音效庫',
        },
        {
          src: require('../public/music/The_End.mp3'),
          pic: '',
          title: 'The End',
          artist: 'Quincas Moreira',
          album: 'You Tube 音效庫',
        },
        {
          src: require('../public/music/The_Long_Night.mp3'),
          pic: '',
          title: 'The Long Night',
          artist: 'Quincas Moreira',
          album: 'You Tube 音效庫',
        },
        {
          src: require('../public/music/Write_You.mp3'),
          pic: '',
          title: 'Write_You',
          artist: 'Joey Pecoraro',
          album: 'You Tube 音效庫',
        }
      ]
    }
  },
  name: 'app',
  components: {
  },
  mounted() {
    this.player.dom = document.querySelector('audio');
    this.replaceAudio(0);
  },
  methods: {
    audioPlay() {
      this.player.dom.play();
    },
    audioPause() {
      this.player.dom.pause();
    },
    replaceAudio(index) {
      this.player.src = this.music[index].src;
      this.player.pic = this.music[index].pic;
      this.player.title = this.music[index].title;
      this.player.artist = this.music[index].artist;
      this.player.album = this.music[index].album;
    }
  },
  computed: {
    
  }
}
</script>

<style lang="scss" scoped>
  @import './assets/scss/_utils.scss';
  @import './assets/scss/main.scss';

</style>
