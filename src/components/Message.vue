<template>
  <div class="message-container">
    {{msg}}
  </div>
</template>


<script>
  export default{
    name: 'Message',
    props:{
      msg: String
    }
  }

</script>


<style scoped>
  .message-container{
    max-width: 300px;
    padding: 15px;
    background: rgb(164, 234, 236);
    border: 2px solid darkblue;
    border-radius: 10px;
    margin: 30px auto;
    font-weight: bold;
    text-align: center;
  }

</style>