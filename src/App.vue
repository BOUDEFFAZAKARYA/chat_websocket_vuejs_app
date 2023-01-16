<script setup>

import {io} from "socket.io-client";


import { ref , onBeforeMount } from "vue";


const socket = io("http://localhost:4000") ;

const messages = ref([]);


onBeforeMount(()=>{
 socket.emit('findAllMessages' , {} , (Response) => {
 messages.value = Response ;

});

socket.on('message' ,(message)=>{

  messages.value.push(message);

} )





})



</script>

<template>

  <div class="chat">

    <div class="chat-container">

      <div  class="messages-container">

        <div  v-for="message in messages">

          [{{ message.name }}] : {{ message.text }}


        </div>


      </div>
    </div>

  </div>

</template>

<style scoped>

</style>
