<script setup>
import Chat from './components/Chat.vue';
import Login from './components/Login.vue';
</script>

<template>
  <main class="main__container">
    <Chat/>
    <Login/>
  </main>
</template>

<style scoped>
.main__container {
  padding: 20px;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
</style>
