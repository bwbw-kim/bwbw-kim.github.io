<script setup>
  import $ from 'jquery';
</script>

<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">
  <link rel="stylesheet" type="text/css" v-bind:href="type">
  <h1 class="gallery_open">Gallery</h1>
  <div class="container">
	
  <div class="gallery">
    <div class="main">
      <div class="main-img">
        <div class="item-img active" data-slide="1"><img class="item-img-size" src="../assets/images/10.jpg"></div>
        <div class="item-img" data-slide="2"><img class="item-img-size" src="../assets/images/11.jpg"></div>
        <div class="item-img" data-slide="3"><img class="item-img-size" src="../assets/images/13.jpg"></div>
        <div class="item-img" data-slide="4"><img class="item-img-size" src="../assets/images/14.jpg"></div>
        <div class="item-img" data-slide="5"><img class="item-img-size" src="../assets/images/10.jpg"></div>
        <div class="item-img" data-slide="6"><img class="item-img-size" src="../assets/images/11.jpg"></div>
        <div class="item-img" data-slide="7"><img class="item-img-size" src="../assets/images/13.jpg"></div>
        <div class="item-img" data-slide="8"><img class="item-img-size" src="../assets/images/14.jpg"></div>
      </div>
      <div class="nutslide">
        <span class="prev"><img class="arrow-size" src="../assets/images/left.png"></span>
        <span class="next"><img  class="arrow-size" src="../assets/images/right.png"></span>
      </div>
    </div><!--cot-1-->
    <div class="list-img">
      <div class="img-thumbnail">
        <img src="../assets/images/10.jpg" class="item-list active list-img-size">
        <img src="../assets/images/11.jpg" class="item-list list-img-size">
        <img src="../assets/images/13.jpg" class="item-list list-img-size">
        <img src="../assets/images/14.jpg" class="item-list list-img-size">
        <img src="../assets/images/10.jpg" class="item-list list-img-size">
        <img src="../assets/images/11.jpg" class="item-list list-img-size">
        <img src="../assets/images/13.jpg" class="item-list list-img-size">
        <img src="../assets/images/14.jpg" class="item-list list-img-size">
      </div><!--img-thumbnail-->
    </div><!--list-img-->
  </div><!--gallery-->
</div>
</template>
<style>


</style>
<script>
  var angle = 0;
  export default {
    data() {
      return { 
        type: "src/css/GalleryDesktop.css"
      }
    },
    created() {
      if(window.innerHeight > window.innerWidth) this.type = "src/css/GalleryMobile.css";
      $(function() {
        $('.item-list').click(function() {
            $('.item-img').removeClass('active');
            $('.item-list').removeClass('active');
            $(this).addClass('active');
            var index = $(this).index() + 1;
            var index2 = $('.item-img:nth-child(' + index + ')').addClass('active');
            $('.main-img').animate({
                left: 0 - index2.position().left
            }, 500);
            var test = $('.item-img.active').index() + 1;
            var a = $('.item-list:nth-child(' + test + ')');
            $('.img-thumbnail').animate({
                left: 0 - (a.position().left / 2)
            }, 300);
        });
        function next() {
            var sau = $('.active').next();
            $('.item-img').removeClass('active');
            $('.item-list').removeClass('active');
            sau.addClass('active');
            if (sau.length == 0) {
                $('.main-img').animate({
                    left: 0 - $('.item-img:first-child').position().left
                }, 500);
                $('.item-img:first-child').addClass('active');
                $('.item-list:first-child').addClass('active');
            } else {
                var hay = $('.item-img.active').attr('data-slide');
                var index5 = $('.item-img:nth-child(' + hay + ')').addClass('active');
                $('.main-img').animate({
                    left: 0 - index5.position().left
                }, 500);
            }
        }
        function prev() {
            var sau = $('.active').prev();
            $('.item-img').removeClass('active');
            $('.item-list').removeClass('active');
            sau.addClass('active');
            if (sau.length == 0) {
                $('.main-img').animate({
                    left: 0 - $('.item-img:last-child').position().left
                }, 500);
                $('.item-img:last-child').addClass('active');
                $('.item-list:last-child').addClass('active');
            }
            var hay = $('.item-img.active').attr('data-slide');
            var index5 = $('.item-img:nth-child(' + hay + ')').addClass('active');
            $('.main-img').animate({
                left: 0 - index5.position().left
            }, 500);
        }
        
        $('.gallery').mouseover(function() {
            clearTimeout(2000);
        });
        $('.gallery').mouseout(function() {
        });
        //click next
        $('.next').click(function() {
            next();
            var test = $('.item-img.active').index() + 1;
            var a = $('.item-list:nth-child(' + test + ')');
            $('.img-thumbnail').animate({
                left: 0 - (a.position().left / 2)
            }, 500);
        });
        //click prev
        $('.prev').click(function() {
            prev();
            var test = $('.item-img.active').index() + 1;
            var a = $('.item-list:nth-child(' + test + ')');
            $('.img-thumbnail').animate({
                left: 0 - (a.position().left / 2)
            }, 500);
        });
    });
    },
    methods: {
    }
  }

</script>