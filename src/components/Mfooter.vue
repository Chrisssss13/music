<template>
    <div class="footer">
        <div class="time">
            <span class="currentTime">00:00</span>
            /
            <span class="totalTime">00:00</span>
        </div>
        <div class="btn" @click="plpa">
            <img src="../pic/播放.png" class="pl" v-if="plorpa" />
            <img src="../pic/暂停 停止.png" class="pa" v-else="plorpa" />
        </div>
        <audio id="audio" :src="musicurl" autoplay="autoplay" loop="loop"></audio>
        <div class="totalProgress" @click="change">
            <div class="currentProgress"></div>
        </div>
        <div class="voice">
            <img src="../pic/音量.png" class="sound" />
            <img src="../pic/音量 静音 黑@2x.png" class="muted" />
        </div>
    </div>
</template>

<script>
    import Bus from '../bus.js'
    import $ from 'jquery'

    export default {
        data() {
            return {
                musicurl: "",
                plorpa: true,
            }
        },
        created() {
            Bus.$on("musicurl", (val) => {
                if (this.musicurl === val) {
                    var audio = document.getElementById('audio');
                    audio.currentTime = 0;
                }
                this.musicurl = val
                this.plorpa = true
                this.plpa()
            })
            Bus.$on("playmusic", (val) => {
                this.plorpa = false
                this.plpa()
            })
        },
        watch: {
            musicurl(newV) {

            }
        },

        methods: {
            plpa() {
                var audio = document.getElementById('audio');
                var totalProgress = $('.totalProgress');
                var currentProgress = $('.currentProgress');
                var currentTime = $('.currentTime');
                var totalTime = $('.totalTime');
                var timer;//计时器
                if (this.musicurl) {
                    if (this.plorpa) {
                        audio.play();//播放音频
                        this.plorpa = false;
                        //计时器实时更改进度条
                        timer = setInterval(function () {

                            let min = parseInt(audio.currentTime / 60);
                            let sec = parseInt(audio.currentTime % 60);
                            if (min < 10) {
                                min = '0' + min;
                            }
                            if (sec < 10) {
                                sec = '0' + sec;
                            }
                            let time = min + ':' + sec
                            currentTime.text(time);
                            min = parseInt(audio.duration / 60);
                            sec = parseInt(audio.duration % 60);
                            if (min < 10) {
                                min = '0' + min;
                            }
                            if (sec < 10) {
                                sec = '0' + sec;
                            }
                            time = min + ':' + sec
                            totalTime.text(time);
                            //更改进度条
                            var ratio = audio.currentTime / audio.duration;
                            currentProgress.css({ 'width': ratio * 100 + '%' });

                        }, 100);
                    } else {
                        audio.pause();//暂停音频
                        //更改播放按钮
                        this.plorpa = true;
                    }
                }
                Bus.$emit("plorpa", this.plorpa)
            },
            change(ev) {
                var audio = document.getElementById('audio');
                var totalProgress = $('.totalProgress');
                var currentProgress = $('.currentProgress');
                var currentTime = $('.currentTime');
                var totalTime = $('.totalTime');
                var timer;//计时器
                //获取百分比
                var ratio = getRatio(ev);
                currentProgress.css({ 'width': ratio * 100 + '%' });
                //更改音频进度条
                audio.currentTime = audio.duration * ratio;

                function getRatio(ev) {
                    //总进度条的实际长度
                    var totawidth = totalProgress[0].offsetWidth;
                    //总进度条的X坐标
                    var totalX = totalProgress.offset().left;
                    //鼠标的x坐标
                    var mouseX = ev.clientX;
                    //求出百分比
                    var ratio = (mouseX - totalX) / totawidth;
                    return ratio;
                }
            }
        }




    }


</script>

<style>
    .footer {
        position: relative;
        background-color: #fff;
        height: 50px;
    }

    .btn {
        position: absolute;
        left: 10px;
        top: 14px;
        height: 30px;
        width: 30px;
        cursor: pointer;
    }

    .pl {
        width: 30px;
        height: 30px;

    }

    .pa {
        width: 30px;
        height: 30px;

    }



    .totalProgress {
        cursor: pointer;
        position: absolute;
        top: 25px;
        left: 160px;
        width: 400px;
        height: 8px;
        background-color: lightgray;
        border-radius: 10px;
    }

    .currentProgress {
        width: 0;
        height: 100%;
        border-radius: 10px;
        background-color: darkgray;
    }

    .time {
        margin-left: 10px;
        position: absolute;
        top: 18px;
        left: 40px;
        color: black;
    }

    .voice {
        position: absolute;
        right: 72px;
        top: 13px;
        height: 30px;
        width: 30px;
        cursor: pointer;
    }

    .sound {
        display: block;
        width: 30px;
        height: 30px;
    }

    .muted {
        display: none;
        width: 30px;
        height: 30px;
    }
</style>