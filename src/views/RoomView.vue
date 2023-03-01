<template>
  <div>
    <h1>Room Chat</h1>

    <div>
      <input v-model="message" type="text" />
      <button @click="send">Send</button>
    </div>

    <div style="margin-top: 10px; text-align: left">
      <div v-for="msg in messageList">
        <h6 style="margin: 0px">{{ msg.username }}</h6>
        <p style="margin: 0px">{{ msg.message }}</p>
        <small>{{ msg.timestamp }}</small>
      </div>
    </div>
  </div>
</template>

<script setup>
import { io } from "socket.io-client";
import { ref, reactive, onMounted, onUnmounted } from "vue";

const socketId = localStorage.getItem("socketId");
// const socket = io(`ws://localhost:3000?id=${socketId}&room=23`);
const socket = io(`ws://103.191.92.211:3000?id=${socketId}&room=23`);
const messageList = reactive([]);

const message = ref("");
const send = () => {
  socket.emit("roomId:23", {
    message: message.value,
    username: localStorage.getItem("username"),
  });
};

socket.on("roomId:23", (data) => {
  console.log(messageList);
  messageList.push(data);
});

onUnmounted(() => {
  socket.close();
});
</script>
