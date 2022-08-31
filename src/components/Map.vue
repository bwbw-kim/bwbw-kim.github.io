<script setup>
  import jQuery from 'jquery'
</script>
<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">

<div class="map_open">Wedding Hall</div>
<br><br>
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3175.5163079468953!2d127.02908761526092!3d37.25918207985657!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x357b4367a2b0ea15%3A0x2c9e245461ae0367!2z7J2067mE7IqkIOyVsOuwsOyEnOuNlCDsiJjsm5A!5e0!3m2!1sko!2skr!4v1661851415543!5m2!1sko!2skr" width="100%" height="300" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
<br><br><br>
<div class="subtitle"> - 대중교통 이용 시</div>
<br>
<div class="content"> 가나다라마바사</div>
<br>
<div class="subtitle"> - 자가용 이용 시</div>
<br>
<div class="content"> 가나다라마바사</div>
<br>
</template>
<style>
  .map_open {
    text-align:center;
    font-size: 200%;
    font-family: 'EngWedding';
    color: black;
  }
  .subtitle{
    font-size: 150%;
    font-family: 'NanumGothic-l';
    text-align: left;
  }
  .content{
    font-size: 130%;
    font-family: 'NanumGothic-l';
    text-align: left;
  }
</style>
<script>

  export default {
    data() {
      return {
      }
    },
    mounted() {
    },
    created() {
      
    },
    methods: {
      
    }
  }

</script>