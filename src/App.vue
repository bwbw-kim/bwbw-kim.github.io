
<script setup>
import Maindoor from './components/Maindoor.vue';
import Greeting from './components/Greeting.vue';
import Gallery from './components/Gallery.vue';
import Contact from './components/Contact.vue';
import Account from './components/Account.vue';
import Map from './components/Map.vue';
import Comment from './components/Comment.vue';
</script>

<template>
  <div style="text-align: center;margin-left:auto;margin-right:auto;">
    <Maindoor />
  </div>
  <br><br><br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;">
    <Greeting />
  </div>
  <br><br><br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Gallery />
  </div>
  <br><br><br><br><br><br><br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:70%">
    <Map />
  </div>
  <br><br><br><br><br><br><br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Contact />
  </div>
  <br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Account />
  </div>
  <!-- <br><br><br><br><br><br><br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%"  >
    <Comment />
  </div> -->
</template>

