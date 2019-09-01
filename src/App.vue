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
                        <a href="javascript:;" class="library-btn">Recently played</a>
                        <a href="javascript:;" class="library-btn">Liked music</a>
                        <a href="javascript:;" class="library-btn">Albums</a>
                        <a href="javascript:;" class="library-btn">Singer</a>
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
                                <a href="javascript:;" class="musiclist-item"
                                    :class="{'current': index === player.index}"
                                    @click.prevent="audioPlayerChoose(index)">
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
                    <div class="member">Member Name</div>
                    <span class="mark">Your Music List</span>
                    <ul class="songlist-list">
                        <li></li>
                    </ul>
                    <div class="youtubeIframe">
                        <youtube :video-id="player.id" width="270" height="152" ref="youtube"
                            :player-vars="youtube.option" @ended="audioPlayerNext" @error="audioPlayerNext"></youtube>
                    </div>
                </div>
            </div>
            <div class="contral">
                <div class="side-left">
                    <div class="audio__title">{{ player.title }}</div>
                    <div class="audio__info">{{ player.artist }}, {{ player.album }}</div>
                </div>
                <div class="side-center">
                    <div class="audio__controlbar">
                        <a href="javascript:;" class="audio__btn prev-btn" @click.prevent="audioPlayerPrev"></a>
                        <a href="javascript:;" class="audio__btn play-btn"
                            :class="{'icon-pause': player.isPause === false}" @click.prevent="audioPlayerCtrl"></a>
                        <a href="javascript:;" class="audio__btn next-btn" @click.prevent="audioPlayerNext"></a>
                    </div>
                    <div class="audio__progressbar">
                        <span class="current-time">{{ getTimeText(player.time) }}</span>
                        <div class="progress">
                            <div class="progress-value" :style="`width:${getPercent}%;`"></div>
                        </div>
                        <span class="duration-time">{{ getTimeText(player.duration) }}</span>
                    </div>
                </div>
                <div class="side-right">
                    <div class="volume">
                        <input class="volume-range" type="range" v-model="player.volume" @mousemove="audioPlayerVolume">
                    </div>
                </div>
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
                    isPause: true,
                    volume: '50'
                },
                youtube: {
                    id: '',
                    option: {
                        controls: 0,
                        rel: 0,
                        fs: 0,
                        iv_load_policy: 3,
                        modestbranding: 1,
                        playsinline: 1
                    },
                },
                videoTimer: null,
                music: [{
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
                    {
                        id: 'lEi_XBg2Fpk',
                        pic: require('../public/img/album/thechainsmokers_allweknow.jpeg'),
                        title: 'All We Know',
                        artist: 'The Chainsmokers',
                        album: 'All We Know',
                        duration: 196
                    },
                    {
                        id: '56WBK4ZK_cw',
                        pic: require('../public/img/album/benny_blanco_friends_keep_secrets.png'),
                        title: 'Eastside',
                        artist: 'Benny Blanco',
                        album: 'Friends Keep Secrets',
                        duration: 175
                    }
                ]
            }
        },
        name: 'app',
        components: {},
        mounted() {
            this.replaceAudio(0);
        },
        methods: {
            audioPlay() {
                // 播放
                console.log('影片播放')
                this.$nextTick(function () {
                    this.videoPlayer.playVideo();
                    this.setVideoTimmer();
                    this.player.isPause = false;
                });
            },
            audioPause() {
                // 暫停
                console.log('影片暫停')
                this.videoPlayer.pauseVideo();
                this.clearVideoTimmer();
                this.player.isPause = true;
            },
            audioStop() {
                // 停止
                console.log('影片停止')
                this.videoPlayer.stopVideo();
                this.clearVideoTimmer();
                this.player.isPause = true;
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
                // 設定播放器計時器
                this.videoTimer = setInterval(() => {
                    // 取得當前時間
                    this.videoPlayer.getCurrentTime().then((result) => {
                        let time = Math.ceil(result);
                        this.player.time = time || 0;
                    });

                    // 取得總時間
                    this.videoPlayer.getDuration().then((result) => {
                        console.log(`總時間：${result}`)
                    });
                }, 1000);
            },
            clearVideoTimmer() {
                // 清除播放器計時器
                clearInterval(this.videoTimer);
            },
            audioPlayerCtrl() {
                // 播放器播放與暫停
                this.player.isPause === true ? this.audioPlay() : this.audioPause();
            },
            audioPlayerNext() {
                // 播放下一首
                let index = this.player.index + 1;
                if (index >= this.music.length) {
                    index = 0;
                }
                this.audioStop();
                this.replaceAudio(index);
                this.audioPlay();
            },
            audioPlayerPrev() {
                // 播放上一首
                let index = this.player.index - 1;
                if (index < 0) {
                    index = this.music.length - 1;
                }
                this.audioStop();
                this.replaceAudio(index);
                this.audioPlay();
            },
            audioPlayerChoose(index) {
                // 選擇播放哪一首
                this.audioStop();
                this.replaceAudio(index);
                this.audioPlay();
            },
            audioPlayerVolume() {
                // 聲音調整
                let value = parseInt(this.player.volume);
                this.videoPlayer.setVolume(value);
            },
            getTimeText(time) {
                // 時間單位轉換
                let minText = '',
                    secText = '';
                let min = Math.floor(time / 60);
                let sec = time % 60;
                min < 10 ? minText = `0${min}` : minText = `${min}`;
                sec < 10 ? secText = `0${sec}` : secText = `${sec}`;
                return `${minText}:${secText}`;
            },
        },
        computed: {
            videoPlayer() {
                return this.$refs.youtube.player
            },
            getPercent() {
                let percent = Math.round((this.player.time / this.player.duration * 100) * 1000) / 1000;
                if (percent >= 100) {
                    percent = 100
                }
                if (percent <= 0) {
                    percent = 0
                }
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
