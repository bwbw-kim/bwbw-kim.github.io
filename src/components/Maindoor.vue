
<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">
  <link rel="stylesheet" type="text/css" v-bind:href="type">
  <br>
  <div class="imgbox">
    <img class="grey" src="../assets/images/9.jpg" alt="Maindoor">
    <div class="main_image_text">
      <br>
      2023년&nbsp;&nbsp;&nbsp;<br>
      3월 11월&nbsp;&nbsp;&nbsp;<br>
      14시&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
      병욱 유정&nbsp;&nbsp;<br>
      결혼합니다<br>
    </div>
  </div>
</template>
<style>

</style>
<script>
  export default {
    data() {
      return { 
        type: "src/css/MainDoorDesktop.css"
      }
    },
    created() {
      if(window.innerHeight > window.innerWidth) this.type = "src/css/MainDoorMobile.css";
    }
  }
</script>