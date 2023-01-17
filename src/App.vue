<script setup>



import { io } from "socket.io-client";


import { ref, onBeforeMount, onMounted, onUpdated } from "vue";


const socket = io("http://localhost:5000");

const messages = ref([]);

const messagetext = ref('');

const name = ref('');

const joined = ref(false);

const TypingDisplay = ref('');


onUpdated(() => {

  socket.emit('findAllMessages', {}, (Response) => {
    messages.value = Response;

    console.log(messages.value);

  });


  socket.on('message', (message) => {

    messages.value.push(message);

  });





});


const join = () => {

  console.log(joined.value);


  console.log("join");

  socket.emit('join', { name: name.value }, () => {

  }



  );

  joined.value = true;


  console.log(joined.value);
};


const sendmessage = () => {

  socket.emit('createMessage', { text: messagetext.value }, () => {
    messagetext.value = '';
  });
  

};


let timeout;

const emittyping = () => {

  socket.emit('typing', { isTyping: true });

  timeout = setTimeout(() => { socket.emit('typing', { isTyping: true }) },
    2000);


};





</script>

<template>

  <div class="chat">

    <div v-if="!joined">

      <form @submit.prevent="join()">

        <label>what's your Name:</label>
        <input v-model="name">
        <button type="submit">join</button>




      </form>


    </div>

    <div class="chat-container" v-else>

      <div class="messages-container">

        <div v-for="message in messages">

          [{{ message.name }}] : {{ message.text }}


        </div>




        <div>



          <form @submit.prevent="sendmessage()">

            <label>message:</label>
            <input v-model="messagetext"  @input="emittyping" >
            <button type="submit">send</button>


          </form>


        </div>







      </div>
    </div>

  </div>

</template>

<style scoped>

</style>
