<template>
    <div class="mbody">
        <div class="songs">
            <ul>
                <MsongsItem v-for="(music, index) in musList" :key="index" :music="music" />
            </ul>
        </div>
        <div class="pic">
            <div class="pict rota" :class="plorpa? ' pause': ''">
                <img src="../pic/唱片.png" class="picbar">
                <img :src="picurl" class="mpic" v-if="picurl">
            </div>
        </div>
        <div class="comments">
            <span class="hot">热门留言</span>
            <ul>
                <McommentsItem v-for="(comment, index) in comments" :key="index" :comment="comment" />
            </ul>

        </div>
        <div class="mv" v-show="playmv" @click="hide">
            <video :src="mvurl" controls="controls" autoplay="autoplay"></video>
        </div>
    </div>
</template>

<script>
    import Bus from '../bus.js'
    import MsongsItem from './MsongsItem.vue'
    import McommentsItem from './McommentsItem.vue'
    var pict = document.querySelector(pict)

    export default {
        data() {
            return {
                musList: [],
                picurl: "",
                plorpa: true,
                comments: [],
                mvurl: "",
                playmv: false
            }
        },
        components: {
            MsongsItem,
            McommentsItem
        },
        methods: {
            hide() {
                this.playmv = false
            }
        },
        watch: {
            plorpa(newV) {
                if (newV === true) {
                    var pict = document.querySelector(pict)
                }
            }
        },
        created() {
            Bus.$on("musicList", (val) => {
                this.musList = val
            })
            Bus.$on("picurl", (val) => {
                this.picurl = val
            })
            Bus.$on("plorpa", (val) => {
                this.plorpa = val
            })
            Bus.$on("comments", (val) => {
                this.comments = val
            })
            Bus.$on("mvurl", (val) => {
                this.mvurl = val
            })
            Bus.$on("playmv", (val) => {
                this.playmv = val
            })
            Bus.$on("musicurl", (val) => {
                this.musicurl = val
            })
        }

    }
</script>

<style>
    .mbody {

        position: relative;
        display: flex;
        height: 400px;
    }

    .mbody .songs {
        flex: 1;
    }

    .mbody .songs ul {
        height: 380px;
        width: 167px;
        margin: 10px;
        overflow-y: auto;
        overflow-x: hidden;
    }

    .mbody .songs ul::-webkit-scrollbar {
        width: 4px;
    }

    .mbody .songs ul::-webkit-scrollbar-thumb {
        border-radius: 10px;
        box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
        background: rgba(0, 0, 0, 0.2);
    }

    .mbody .songs ul::-webkit-scrollbar-track {
        box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
        border-radius: 0;
        background: rgba(0, 0, 0, 0.1);
    }



    .mbody .pic {
        flex: 2;
        position: relative;
        border-left: 1px solid #666;
        border-right: 1px solid #666;
    }

    .mbody .pic .pict {
        position: absolute;
        height: 300px;
        width: 300px;
        left: 50%;
        top: 50%;
        margin-left: -150px;
        margin-top: -150px;

    }

    .mbody .pic .pict:hover {
        animation-play-state: paused;
    }


    .mbody .pic .picbar {

        height: 300px;

    }

    .mbody .pic .mpic {
        position: absolute;
        height: 150px;
        width: 150px;
        border-radius: 75px;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
    }

    .rota {
        animation: ro 10s infinite linear;

    }

    .pause {
        animation-play-state: paused;
    }

    @keyframes ro {
        0% {
            transform: rotate(0deg);
        }

        100% {
            transform: rotate(360deg);
        }
    }

    .mbody .comments {
        flex: 1;

    }

    .mbody .comments .hot {
        font-weight: 700;
        margin-left: 10px;
    }

    .mbody .comments ul {
        height: 380px;
        overflow-y: auto;
        overflow-x: hidden;
    }

    .mbody .comments ul::-webkit-scrollbar {
        width: 4px;
    }

    .mbody .commentss ul::-webkit-scrollbar-thumb {
        border-radius: 10px;
        box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
        background: rgba(0, 0, 0, 0.2);
    }

    .mbody .comments ul::-webkit-scrollbar-track {
        box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
        border-radius: 0;
        background: rgba(0, 0, 0, 0.1);
    }

    .mv {
        height: 450px;
        width: 700px;
        position: absolute;
        background: black;
        z-index: 1;
    }

    .mv video {
        width: 100%;
        height: 100%;
    }
</style>