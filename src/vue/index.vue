<template>
    <div class="hj-music">
        <div class="music-wrapper">
            <div
                class="music-bg"
                :style="{backgroundImage: `url(${musicObj.musicCover})`}"
            />
            <div class="music-content">
                <img
                    :src="musicObj.musicCover"
                    alt=""
                    :class="['music-cover', isPlay ? 'active' : '']"
                >
                <div class="music-control">
                    <div class="time-show">
                        <span>{{ realMusicTime }}</span>
                        <span>{{ musicObj.musicName }}</span>
                        <span>{{ totalMusicTime }}</span>
                    </div>
                    <div
                        ref="bar"
                        class="music-bar"
                        @click="handClickBar"
                    >
                        <span
                            ref="slid"
                            class="pro-bar"
                            @click="handClickBar"
                        />
                    </div>
                    <div class="control-action">
                        <img
                            :src="prevIcon"
                            alt=""
                            class="svg-icon"
                            @click="prevSong"
                        >

                        <img
                            v-if="!isPlay"
                            :src="playIcon"
                            alt=""
                            class="svg-icon"
                            @click="playOrStopMusic"
                        >
                        <img
                            v-else
                            :src="pauseIcon"
                            alt=""
                            class="svg-icon"
                            @click="playOrStopMusic"
                        >

                        <img
                            :src="nextIcon"
                            alt=""
                            class="svg-icon"
                            @click="nextSong"
                        >
                    </div>
                </div>
            </div>
        </div>
        <audio
            ref="music"
            :src="musicObj.musicUrl"
            autoplay
        />
        <slot />
    </div>
</template>

<script>
export default {
    name: 'HjVueMusic',
    props: {
        musicData: {
            type: Array,
            default: () => {
                return [];
            },
            require: true
        }
    },
    data() {
        return {
            isPlay: false,
            realMusicTime: '00:00',
            totalMusicTime: '00:00',
            musicObj: {},
            musicDataList: [],
            timeOut: null,
            nextIcon:
                'data:image/png;base64,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',
            pauseIcon:
                'data:image/png;base64,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',
            playIcon:
                'data:image/png;base64,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',
            prevIcon:
                'data:image/png;base64,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'
        };
    },
    watch: {
        musicData: function (newVal, oldVal) {
            console.log(oldVal);
            this.musicDataList = newVal;
        }
    },
    mounted() {
        this.musicObj = this.musicData[0];
    },
    methods: {
        // 暂停或者播放;
        playOrStopMusic() {
            if (this.isPlay) {
                this.$refs.music.pause();
                this.isPlay = false;
            } else {
                this.$refs.music.play();
                this.isPlay = true;
            }
            this.musicTotalTime();
            this.watchMusicTime();
        },
        // 计算音频时间
        musicTotalTime() {
            const time = this.$refs.music.duration;
            // 分钟
            let minutes = parseInt(time / 60);
            if (minutes < 10) {
                minutes = `0${minutes}`;
            }
            // 秒
            let seconds = Math.round(time % 60);
            if (seconds < 10) {
                seconds = `0${seconds}`;
            }
            this.totalMusicTime = `${minutes}:${seconds}`;
        },
        // 获取音乐当前播放进度
        musicCurrentTime(time) {
            let minutes = parseInt(time / 60);
            if (minutes < 10) {
                minutes = `0${minutes}`;
            }
            // 秒
            let seconds = Math.round(time % 60);
            if (seconds < 10) {
                seconds = `0${seconds}`;
            }
            this.realMusicTime = `${minutes}:${seconds}`;
        },
        // 监听音乐播放
        watchMusicTime() {
            this.$refs.music.addEventListener(
                'timeupdate',
                () => {
                    const currentTime = this.$refs.music.currentTime;
                    this.musicCurrentTime(currentTime);
                    this.$nextTick(() => {
                        this.handMusicBar();
                    });
                },
                false
            );
            // 播放完毕
            this.$refs.music.addEventListener('ended', () => {
                this.nextSong(); // 播放下一首
            });
        },
        // 处理进度条
        handMusicBar() {
            const slid = this.$refs.slid;
            const duration = this.$refs.music.duration;
            const x = `${((this.$refs.music.currentTime / duration) * 100).toFixed(2)}%`;
            slid.style.width = x;
        },
        // 处理点击进度条事件
        handClickBar(e) {
            const barTotalWidth = this.$refs.bar.offsetWidth; // bar 总宽度
            const rect = e.target.getBoundingClientRect(); // 元素右边距离页面边距的距离 返回上下左右
            const length = e.pageX - rect.left;
            this.$refs.music.currentTime = (length / barTotalWidth) * this.$refs.music.duration; // 计算播放时间 位置百分比*总时间
            this.$nextTick(() => {
                this.$refs.music.play();
                this.isPlay = true;
            });
        },
        // 播放上一首
        prevSong() {
            if (this.musicData.length === 1) {
                this.musicObj = this.musicData[0];
            } else {
                const musicId = this.musicObj.id;
                const len = this.musicData.length - 1;
                const idx = this.musicData.findIndex(value => value.id === musicId);
                if (idx !== -1) {
                    if (idx === 0) {
                        this.musicObj = this.musicData[len];
                    } else {
                        this.musicObj = this.musicData[idx - 1];
                    }
                }
            }
            clearTimeout(this.timeOut);
            this.timeOut = setTimeout(() => {
                this.$refs.music.play();
                this.isPlay = true;
                this.musicTotalTime();
                this.watchMusicTime();
            }, 100);
        },
        nextSong() {
            if (this.musicData.length === 1) {
                this.musicObj = this.musicData[0];
            } else {
                const musicId = this.musicObj.id;
                const len = this.musicData.length - 1;
                const idx = this.musicData.findIndex(value => value.id === musicId);
                if (idx !== -1) {
                    if (idx === len) {
                        this.musicObj = this.musicData[0];
                    } else {
                        this.musicObj = this.musicData[idx + 1];
                    }
                }
            }
            clearTimeout(this.timeOut);
            this.timeOut = setTimeout(() => {
                this.$refs.music.play();
                this.isPlay = true;
                this.musicTotalTime();
                this.watchMusicTime();
            }, 100);
        }
    }
};
</script>

<style lang="scss" scoped>
.music-wrapper {
    position: relative;
    overflow: hidden;
    width: 500px;
    height: 100px;
    box-sizing: border-box;
    .music-bg {
        width: 100%;
        height: 100%;
        border-radius: 15px;
        background-position: center;
        background-size: cover;
        -webkit-filter: blur(2px);
        -moz-filter: blur(2px);
        -o-filter: blur(2px);
        -ms-filter: blur(2px);
        filter: blur(2px);
    }
    .music-content {
        position: absolute;
        top: 0;
        left: 0;
        display: flex;
        width: 100%;
        height: 100%;
        box-sizing: border-box;
        align-items: center;
        justify-content: space-between;
        padding: 0 20px;
        .music-cover {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            animation: coverRotate 10s linear infinite forwards;
            animation-play-state: paused; // 暂定动画
            &.active {
                animation-play-state: running; // 运行动画
            }
        }
        .music-control {
            flex: 1;
            margin-left: 20px;
            .time-show {
                display: flex;
                justify-content: space-between;
                color: #fff;
            }
            .music-bar {
                display: flex;
                overflow: hidden;
                width: 100%;
                height: 6px;
                border-radius: 10px;
                margin-top: 8px;
                background-color: #fff;
                .pro-bar {
                    display: inline-block;
                    width: 0;
                    height: 6px;
                    background-color: rgb(82, 66, 189);
                }
            }
            .control-action {
                display: flex;
                width: 40%;
                align-items: center;
                justify-content: space-between;
                margin: 0 auto;
                margin-top: 8px;
                .svg-icon {
                    width: 32px;
                    height: 32px;
                }
            }
        }
    }
}
// .cover-rotate {
//   animation: coverRotate 10s linear infinite forwards;
// }
@keyframes coverRotate {
    from {
        transform: rotate(0);
    }
    to {
        transform: rotate(360deg);
    }
}
</style>
