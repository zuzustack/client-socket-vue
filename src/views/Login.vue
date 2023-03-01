<template>
  <div>
    <h1>Login</h1>
    <input type="text" v-model="username" placeholder="username" />
    <button @click="connect">Connect</button>
  </div>
</template>

<script setup>
import router from "../router"
import { io } from "socket.io-client";
import { ref, reactive, onMounted, onUnmounted } from "vue";

const username = ref("");
// const socket = io("ws://localhost:3000/login");
const socket = io("ws://103.191.92.211:3000/login");


const connect = () => {
  socket.emit("login", username.value);

  socket.on("login", (data) => {
    alert(data.status);
    localStorage.setItem('socketId', data.id)
    localStorage.setItem('username', username.value)
    router.push("/room")
  });
};

onUnmounted(() => {
  socket.close();
});
</script>
