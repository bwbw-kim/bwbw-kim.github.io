<script setup>
  import jQuery from 'jquery'
</script>
<template>
  <link rel="stylesheet" type="text/css" href="src/css/font.css">
  댓글 
</template>
<style>
  
</style>
<script>
  export default {
    data() {
      return { 
        type: "src/css/Comment.css"
      }
    },
    created() {
      if(window.innerHeight > window.innerWidth) this.type = "src/css/Comment.css";
      
    },
    methods: {
      
    }
  }

</script>