<script setup>
  import $ from 'jquery';
</script>
<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">
  <link rel="stylesheet" type="text/css" v-bind:href="type">
  <h1 class="gallery_open">Contact</h1>
  <br><br><br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">신랑</div>
    <div class="contact-box-item-b">김병욱</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-8903-4703'"><img class="icon-size" src="../assets/images/telephoneblue.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-8903-4703'"><img class="icon-size" src="../assets/images/msgblue.png"></a>
    <a class="icon-width" href="http://qr.kakao.com/talk/r9qb1GK42s6dp1fYuMrf_gIHUaQ-"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">신부</div>
    <div class="contact-box-item-b">박유정</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-5061-0215'"><img class="icon-size" src="../assets/images/telephonered.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-5061-0215'"><img class="icon-size" src="../assets/images/msgred.png"></a>
    <a class="icon-width" href="http://qr.kakao.com/talk/r9qb1GK42s6dp1fYuMrf_gIHUaQ-"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br><br><br>
  <div class="contact-subtitle">신랑측 혼주</div>
  <br><br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">아버지</div>
    <div class="contact-box-item-b">김용현</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-5523-4703'"><img class="icon-size" src="../assets/images/telephoneblack.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-5523-4703'"><img class="icon-size" src="../assets/images/msgblack.png"></a>
    <a class="icon-width" href="#"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">어머니</div>
    <div class="contact-box-item-b">홍은혜</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-4522-4703'"><img class="icon-size" src="../assets/images/telephoneblack.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-4522-4703'"><img class="icon-size" src="../assets/images/msgblack.png"></a>
    <a class="icon-width" href="#"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br><br><br>
  <div class="contact-subtitle">신부측 혼주</div>
  <br><br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">아버지</div>
    <div class="contact-box-item-b">박순현</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-2006-2697'"><img class="icon-size" src="../assets/images/telephoneblack.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-2006-2697'"><img class="icon-size" src="../assets/images/msgblack.png"></a>
    <a class="icon-width" href="#"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br>
  <div class="contact-box">
    <div class="spacer10"></div> <!-- Spacer -->
    <div class="contact-box-item">어머니</div>
    <div class="contact-box-item-b">신향숙</div>
    <div class="spacer10"></div> <!-- Spacer -->
    <a class="icon-width" onclick="document.location.href='tel:010-2881-6402'"><img class="icon-size" src="../assets/images/telephoneblack.png"></a>
    <a class="icon-width" onclick="document.location.href='sms:010-2881-6402'"><img class="icon-size" src="../assets/images/msgblack.png"></a>
    <a class="icon-width" href="#"><img class="icon-size" src="../assets/images/kakao-talk.png"></a>
  </div>
  <br>
  <hr class="cover-container">
</template>
<style>


</style>
<script>
  var angle = 0;
  export default {
    data() {
      return { 
        type: "src/css/ContactDesktop.css"
      }
    },
    created() {
      if(window.innerHeight > window.innerWidth) this.type = "src/css/ContactMobile.css";
    },
    methods: {
    }
  }

</script>