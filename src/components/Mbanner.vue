<template>
    <div class="banner">
        <span>悦听</span>
        <div class="search">
            <input type="text" v-model="query" @keyup.enter="searchMusic">
            <span class="iconfont icon-icon-search" @click="searchMusic"></span>
        </div>
    </div>
</template>

<script>
    import Bus from '../bus.js'
    export default {
        data() {
            return {
                query: "",
                musicList: []
            }
        },
        methods: {
            searchMusic() {
                this.$axios.get("https://autumnfish.cn/search?keywords=" + this.query)
                    .then(function (response) {
                        this.musicList = response.data.result.songs
                        Bus.$emit("musicList", this.musicList)
                    }.bind(this), function (err) { })
            }
        }
    }
</script>

<style>
    .banner {
        position: relative;
        width: 100%;
        height: 50px;
        background-color: skyblue;
    }

    .banner span {
        height: 50px;
        font-size: 20px;
        color: #fff;
        line-height: 50px;
        margin-left: 30px;
    }

    .banner .search {
        position: absolute;
        right: 20px;
        top: 5px;
        height: 40px;
        width: 200px;
        border-radius: 25px;
        background-color: lightblue;
    }

    .banner .search input {
        position: absolute;
        height: 100%;
        width: 150px;
        background-color: transparent;
        border-style: none;
        margin-left: 20px;
    }

    .banner .search input:focus {
        outline-style: none;
    }

    .banner .search button {
        background-color: transparent;
        border-style: none;
    }

    .icon-icon-search::before {
        position: absolute;
        right: 5px;
        top: -3px;
        font-size: 30px;
        color: darkblue;
    }
</style>