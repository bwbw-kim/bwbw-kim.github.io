
<script setup>
import Maindoor from './components/Maindoor.vue';
import Greeting from './components/Greeting.vue';
import Gallery from './components/Gallery.vue';
import Contact from './components/Contact.vue';
import Account from './components/Account.vue';
import Map from './components/Map.vue';
</script>

<template>
  <div style="text-align: center;margin-left:auto;margin-right:auto;">
    <Maindoor />
  </div>
  <div style="text-align: center;margin-left:auto;margin-right:auto;">
    <Greeting />
  </div>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Gallery />
  </div>
  <br><br>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Map />
  </div>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Contact />
  </div>
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%">
    <Account />
  </div>
  
  <div style="text-align: center;margin-left:auto;margin-right:auto;width:75%"  >
    댓글
  </div>
</template>

