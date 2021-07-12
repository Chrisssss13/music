<template>
  <div class="bg">
    <Mbanner />
    <Mbody />
    <Mfooter />
  </div>
</template>

<script>
  import Mbanner from './components/Mbanner.vue'
  import Mbody from './components/Mbody.vue'
  import Mfooter from './components/Mfooter.vue'

  export default {

    components: {
      Mbanner,
      Mbody,
      Mfooter
    }
  }
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    list-style: none;
  }


  .bg {
    width: 700px;
    height: 500px;
    margin: 100px auto;
    background: url("../src/pic/bg.jpg");
  }
</style>