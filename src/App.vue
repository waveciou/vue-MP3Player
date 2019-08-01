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
            <img :src="player.pic" :alt="player.title">
          </figure>
        </div>
        <div class="side-center">
          <div class="banner-block">
            <img :src="require('../public/img/taylor-swift-ranking.png')" alt="taylor swift">
          </div>
          <div class="music-block">
            <h2>Top Song</h2>
            <ul class="musiclist-list">
              <li v-for="(item, index) in music" :key="index">
                <a href="javascript:;" class="musiclist-item">
                  <div class="ms__info">
                    <figure class="ms__image">
                      <img :src="item.pic" :alt="item.title">
                    </figure>
                    <div class="ms__heading">
                      <span class="ms__index">{{ index + 1 }}</span>
                      <span class="ms__name">{{ item.title }}</span>
                    </div>
                  </div>
                  <span class="ms__duration">{{getTimeText(item.duration)}}</span>
                </a>
              </li>
            </ul>
          </div>
        </div>
        <div class="side-right">
          <div class="member"></div>
          <span class="mark">Your Music List</span>
          <ul class="songlist-list">
            <li></li>
          </ul>
          <youtube :video-id="player.id" width="270" height="152" ref="youtube" :player-vars="youtube" @ended="audioStop" @paused="audioPause"></youtube>
        </div>
      </div>
      <div class="contral">
        <div class="side-left">
          <div class="audio__title">{{ player.title }}</div>
          <div class="audio__info">{{ player.artist }}, {{ player.album }}</div>
        </div>
        <div class="side-center">
          <div class="audio__controlbar">
            <a href="javascript:;" class="audio__btn prev-btn"></a>
            <a href="javascript:;" class="audio__btn play-btn"></a>
            <a href="javascript:;" class="audio__btn next-btn"></a>
          </div>
          <div class="audio__progressbar">
            <span class="current-time">{{ getTimeText(player.time) }}</span>
            <div class="progress">
              <div class="progress-value" :style="`width:${getPercent}%;`"></div>
            </div>
            <span class="duration-time">{{ getTimeText(player.duration) }}</span>
          </div>
        </div>
        <div class="side-right"></div>
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
        time: 0,
        duration: 0,
        index: 0,
      },
      youtube: {
        id: '',
        option: {
          rel: 0,
          fs: 1,
          iv_load_policy: 3,
          modestbranding: 1,
          playsinline: 1
        },
        events: {
          
        }
      },
      videoTimer: null,
      music: [
        {
          id: 'w1oM3kQpXRo',
          pic: require('../public/img/album/taylorswift_red.png'),
          title: 'Everything Has Changed',
          artist: 'Taylor Swift',
          album: 'Red',
          duration: 252
        },
        {
          id: '-CmadmM5cOk',
          pic: require('../public/img/album/taylorswift_1989.jpg'),
          title: 'Style',
          artist: 'Taylor Swift',
          album: '1989',
          duration: 243
        },
        {
          id: 'e-ORhEE9VVg',
          pic: require('../public/img/album/taylorswift_1989.jpg'),
          title: 'Blank Space',
          artist: 'Taylor Swift',
          album: '1989',
          duration: 273
        },
        {
          id: 'KINfQbfZwik',
          pic: require('../public/img/album/onerepublic_native.jpg'),
          title: 'I Lived',
          artist: 'One Republic',
          album: 'Native',
          duration: 235
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
      // 播放
      console.log('影片播放')
      this.$nextTick(function() {
        this.videoPlayer.playVideo();
        this.setVideoTimmer();
      });
    },
    audioPause() {
      // 暫停
      console.log('影片暫停')
      this.videoPlayer.pauseVideo();
      this.clearVideoTimmer();
    },
    audioStop() {
      // 停止
      console.log('影片停止')
      this.videoPlayer.stopVideo();
      this.clearVideoTimmer();
    },
    replaceAudio(index) {
      // 替換音樂
      this.player.index = index;

      this.player.id = this.music[index].id;
      this.player.pic = this.music[index].pic;
      this.player.title = this.music[index].title;
      this.player.artist = this.music[index].artist;
      this.player.album = this.music[index].album;
      this.player.duration = this.music[index].duration;

      this.youtube.id = this.player.id;
    },
    setVideoTimmer() {
      this.videoTimer = setInterval(() => {

        // 取得當前時間
        this.videoPlayer.getCurrentTime().then((result) => {
          let time = Math.ceil(result);
          this.player.time = time || 0;
        });

        // 取得總時間
        // this.videoPlayer.getDuration().then((result) => {
        //   console.log(result)
        // });

      }, 1000);
    },
    clearVideoTimmer() {
      clearInterval(this.videoTimer);
    },
    getTimeText(time) {
      let minText = '', secText = '';
      let min = Math.floor(time / 60);
      let sec = time % 60;
      min < 10 ? minText = `0${min}` : minText = `${min}`;
      sec < 10 ? secText = `0${sec}` : secText = `${sec}`;
      return `${minText}:${secText}`;
    }
  },
  computed: {
    videoPlayer() {
      return this.$refs.youtube.player
    },
    getPercent() {
      let percent = Math.floor(this.player.time / this.player.duration * 100);
      if(percent >= 100) { percent = 100 }
      if(percent <= 0) { percent = 0 }
      return percent;
    },
  },
}
</script>

<style lang="scss" scoped>
  @import './assets/scss/_utils.scss';
  @import './assets/scss/main.scss';
  @import './assets/scss/musiclist.scss';
  @import './assets/scss/audio.scss';

</style>
