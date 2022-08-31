<script setup>
  import jQuery from 'jquery'
</script>
<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">
  <link rel="stylesheet" type="text/css" href="src/css/demo.min.css">
  <link rel="stylesheet" type="text/css" href="src/css/tabler-flags.min.css">
  <link rel="stylesheet" type="text/css" href="src/css/tabler-payments.min.css">
  <link rel="stylesheet" type="text/css" href="src/css/tabler-vendors.min.css">
  <link rel="stylesheet" type="text/css" href="src/css/tabler.min.css">
  <link rel="stylesheet" type="text/css" v-bind:href="type">
  <br><br>
  <div class="title">신랑 신부에게 마음 전하는 곳</div>

  <a href="#0" class="cd-popup-trigger">신랑 신부에게 마음 전하기</a>

  <div class="cd-popup" role="alert">
    <div class="cd-popup-container">
      <div class="popup-title">마음 전하기</div>
      <ul class="cd-buttons">
        <div class="row">
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
          <div class="col-6" style="text-align:left;">
            <div class="popup-subtitle">신랑 김병욱 [우리은행]</div>
          </div>
          <div class="col-4" style="text-align:right;">
            <div class="popup-subtitle">예금주 : 김병욱</div>
          </div>
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
        </div>
        <div class="row">
          <div class="col-1"></div>
          <div class="col-7">
            <input id="male" type="text" class="form-control" name="example-disabled-input" placeholder="Disabled..." value="1002-352-948502" disabled="">
          </div>
          <div class="col-3">
            <button @click="doCopy('1002-352-948502')" class="btn btn-outline-dark w-100">
              복사하기
            </button>
          </div>
          <div class="col-1"></div>
        </div>
        <br>
        <div class="row">
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
          <div class="col-6" style="text-align:left;">
            <div class="popup-subtitle">신부 박유정 [부산은행]</div>
          </div>
          <div class="col-4" style="text-align:right;">
            <div class="popup-subtitle">예금주 : 박유정</div>
          </div>
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
        </div>
        <div class="row">
          <div class="col-1"></div>
          <div class="col-7">
            <input id="female" type="text" class="form-control" name="example-disabled-input" placeholder="Disabled..." value="042-1208-89276" disabled="">
          </div>
          <div class="col-3">
            <button @click="doCopy('042-1208-89276')" class="btn btn-outline-dark w-100">
              복사하기
            </button>
          </div>
          <div class="col-1"></div>
        </div>
        <br>
        <div class="row">
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
          <div class="col-6" style="text-align:left;">
            <div class="popup-subtitle">신랑 부모님 [00은행]</div>
          </div>
          <div class="col-4" style="text-align:right;">
            <div class="popup-subtitle">예금주 : 000</div>
          </div>
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
        </div>
        <div class="row">
          <div class="col-1"></div>
          <div class="col-7">
            <input id="male-family" type="text" class="form-control" name="example-disabled-input" placeholder="Disabled..." value="000000000000" disabled="">
          </div>
          <div class="col-3">
            <button @click="doCopy('000000000000')" class="btn btn-outline-dark w-100">
              복사하기
            </button>
          </div>
          <div class="col-1"></div>
        </div>
        <br>
        <div class="row">
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
          <div class="col-6" style="text-align:left;">
            <div class="popup-subtitle">신부 부모님 [00은행]</div>
          </div>
          <div class="col-4" style="text-align:right;">
            <div class="popup-subtitle">예금주 : 000</div>
          </div>
          <div class="col-1">
            <div class="popup-subtitle"></div>
          </div>
        </div>
        <div class="row">
          <div class="col-1"></div>
          <div class="col-7">
            <input id="female-family" type="text" class="form-control" name="example-disabled-input" placeholder="Disabled..." value="000000000" disabled="">
          </div>
          <div class="col-3">
            <button @click="doCopy('000000000')" class="btn btn-outline-dark w-100">
              복사하기
            </button>
          </div>
          <div class="col-1"></div>
        </div>
        <br><br>
      </ul>
      <a href="#0" class="cd-popup-close img-replace">Close</a>
    </div> <!-- cd-popup-container -->
  </div> <!-- cd-popup -->
  <hr class="cover-container">
  <br>
</template>
<style>

</style>
<script>
  export default {
    data() {
      return { 
        type: "src/css/Account.css"
      }
    },
    created() {
      if(window.innerHeight > window.innerWidth) this.type = "src/css/Account.css";
      jQuery(document).ready(function($){
        //open popup
        $('.cd-popup-trigger').on('click', function(event){
          event.preventDefault();
          $('.cd-popup').addClass('is-visible');
        });
        
        //close popup
        $('.cd-popup').on('click', function(event){
          if( $(event.target).is('.cd-popup-close') || $(event.target).is('.cd-popup') ) {
            event.preventDefault();
            $(this).removeClass('is-visible');
          }
        });
        //close popup when clicking the esc keyboard button
        $(document).keyup(function(event){
            if(event.which=='27'){
              $('.cd-popup').removeClass('is-visible');
            }
          });
      });
    },
    methods: {
      doCopy(text) {
        var textArea = document.createElement("textarea");
        textArea.value = text;
        textArea.style.top = "0";
        textArea.style.left = "0";
        textArea.style.position = "fixed";
        document.body.appendChild(textArea);
        textArea.focus();
        textArea.select();
        try {
          var successful = document.execCommand('copy');
          var msg = successful ? 'successful' : 'unsuccessful';
          console.log('Fallback: Copying text command was ' + msg);
        } catch (err) {
          console.error('Fallback: Oops, unable to copy', err);
        }

        document.body.removeChild(textArea);
        alert(text + "이(가) 복사 되었습니다. ")
      }
    }
  }

</script>