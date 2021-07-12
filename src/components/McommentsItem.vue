<template>
    <li class="coI clearfix">
        <span class="png">
            <img :src="comment.user.avatarUrl">
        </span>
        <span class="co">
            <span class="name">{{comment.user.nickname}}</span>
            <br>
            <span class="comm">{{comment.content}}</span>
        </span>
    </li>
</template>

<script>
    export default {
        props: {
            comment: Object,
            index: Number
        },
        data() {
            return {
                headurl: ""
            }
        }
    }
</script>

<style>
    .coI {
        position: relative;
        padding-bottom: 20px;
    }

    .clearfix:after {
        content: "020";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }


    .png {
        float: left;
        width: 50px;
        height: 30px;
    }

    .png img {
        width: 30px;
        margin-left: 10px;
        border-radius: 15px;
    }

    .co {
        float: left;
        width: 110px;
    }

    .co .name {
        width: 100%;
        font-size: 16px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .co .comm {
        width: 100%;
        font-size: 12px;
        word-wrap: break-word;
    }
</style>