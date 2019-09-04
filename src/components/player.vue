<template>
    <div class="contral">
        <div class="side-left">
            <div class="audio__title">{{ player.title }}</div>
            <div class="audio__info">{{ player.artist }}, {{ player.album }}</div>
        </div>
        <div class="side-center">
            <div class="audio__controlbar">
                <a href="javascript:;" class="audio__btn prev-btn" @click.prevent="audioPlayerPrev"></a>
                <a href="javascript:;" class="audio__btn play-btn" :class="{'icon-pause': player.isPause === false}"
                    @click.prevent="audioPlayerCtrl"></a>
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
</template>

<script>
    export default {
        data() {
            return {

            }
        },
        name: 'Player',
        props: {
            player: Object
        },
        methods: {
            audioPlayerCtrl() {
                this.$emit('audioPlayerCtrl');
            },
            audioPlayerNext() {
                this.$emit('audioPlayerNext');
            },
            audioPlayerPrev() {
                this.$emit('audioPlayerPrev');
            },
            audioPlayerVolume() {
                this.$emit('audioPlayerVolume');
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
        }
    }
</script>

<style lang="scss" scoped>
    @import '../assets/scss/_utils.scss';

    .contral {
        width: 100%;
        height: 135px;
        padding: 15px 20px;
        background-color: $color-gray-light;
        display: flex;
        align-items: center;
        justify-content: space-between;

        .side-left {
            width: 25%;
        }

        .side-center {
            width: 50%;
        }

        .side-right {
            width: 25%;
        }
    }

    .audio__title {
        font-size: 22px;
        font-weight: bold;
        color: $color-black;
        margin-bottom: 10px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

    .audio__info {
        font-size: 18px;
        color: $color-gray;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

    .audio__controlbar {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 15px;
    }

    .audio__btn {
        width: 20px;
        height: 20px;
        display: block;
        margin: 0px 15px;
        background: {
            repeat: no-repeat;
            position: center;
            size: contain;
        }

        &.play-btn {
            width: 35px;
            height: 35px;
            border: {
                width: 1px;
                color: $color-black;
                style: solid;
                radius: 100%;
            }
            background-image: url('../../public/img/play.svg');
            background-size: 60%;

            &.icon-pause {
                background-image: url('../../public/img/pause.svg');
            }
        }

        &.prev-btn {
            background-image: url('../../public/img/previous.svg');
        }

        &.next-btn {
            background-image: url('../../public/img/next.svg');
        }
    }

    .audio__progressbar {
        display: flex;
        align-items: center;
        justify-content: space-between;

        span {
            display: block;
            font-size: 16px;
            padding: 0px 10px;
        }
    }

    // 時間軸進度條
    .progress {
        width: 100%;
        height: 7px;
        background-color: $color-gray-3;
        position: relative;
        overflow: hidden;
    }

    .progress-value {
        height: 100%;
        background-color: $color-mint;
        // transition: width 1s linear;
        position: absolute;
        top: 0%;
        left: 0%;
    }

    // 聲音控制
    .volume {
        display: flex;
        align-items: center;
        justify-content: flex-end;

        &::before {
            content: '';
            width: 25px;
            height: 25px;
            display: block;
            margin-right: 10px;
            background: {
                image: url('../../public/img/volume.svg');
                repeat: no-repeat;
                position: center;
                size: contain;
            }
        }
    }

    .volume-range {
        -webkit-appearance: none;
        width: 150px;
        height: 7px;
        border-radius: 5px;
        background: $color-gray-3;
        outline: none;
        padding: 0;
        margin: 0;
        cursor: pointer;

        &::-webkit-slider-thumb {
            appearance: none;
            width: 10px;
            height: 10px;
            border-radius: 100%;
            background-color: $color-gray;
        }
    }
</style>