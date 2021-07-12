<template>
    <li class="soI">
        <button class="play iconfont icon-yinleicon_huaban1fuben10" @click="playMusic"></button>
        <span>{{music.name}}</span>
        <button class="playmv iconfont icon-mv" v-if="music.mvid" @click="playMv"></button>
    </li>
</template>

<script>
    import Bus from '../bus.js'
    export default {
        props: {
            music: Object,
            index: Number
        },
        data() {
            return {
                musicurl: "",
                picurl: "",
                comments: [],
                mvurl: "",
                playmv: false,
                playmusic: true
            }

        },
        methods: {
            playMusic() {
                this.$axios.get("https://autumnfish.cn/song/url?id=" + this.music.id)
                    .then(function (response) {
                        // console.log(respnse)
                        this.musicurl = response.data.data[0].url
                        Bus.$emit("musicurl", this.musicurl)
                    }.bind(this), function (err) { })
                this.$axios.get("https://autumnfish.cn/song/detail?ids=" + this.music.id)
                    .then(function (response) {
                        this.picurl = response.data.songs[0].al.picUrl
                        Bus.$emit("picurl", this.picurl)
                    }.bind(this), function (err) { })
                this.$axios.get("https://autumnfish.cn/comment/hot?type=0&id=" + this.music.id)
                    .then(function (response) {
                        this.comments = response.data.hotComments
                        Bus.$emit("comments", this.comments)
                    }.bind(this), function (err) { })
            },
            playMv() {
                this.playmv = true
                this.$axios.get("https://autumnfish.cn/mv/url?id=" + this.music.mvid)
                    .then(function (response) {
                        this.mvurl = response.data.data.url
                        Bus.$emit("mvurl", this.mvurl)
                        Bus.$emit("playmv", this.playmv)
                        Bus.$emit("playmusic", this.playmusic)
                    }.bind(this), function (err) { })
            }

        }

    }
</script>

<style>
    .soI {
        position: relative;
        width: 167px;
        height: 40px;

    }

    .soI:nth-child(2n) {
        background: lightyellow;
    }

    .soI .play {
        background-color: transparent;
        border-style: none;
        position: absolute;
        cursor: pointer;
        left: 5px;
        top: 8px;
    }

    .soI span {
        position: absolute;
        left: 25px;
        top: 5px;
        height: auto;
        width: 100px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .soI .playmv {
        background-color: transparent;
        border-style: none;
        position: absolute;
        cursor: pointer;
        right: 10px;
        top: 8px;
    }

    .icon-mv:before {
        position: absolute;
        top: -8px;
        right: -2px;
        font-size: 30px;
    }
</style>