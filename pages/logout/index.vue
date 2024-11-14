<template>
  <div>
    <h2>Logout Page</h2>
    <button @click="handleLogout">Logout</button>
  </div>
</template>

<script setup>
const handleLogout = () => {
  // Logic for handling logout goes here
};
</script>

<style scoped>
button {
  background-color: #dc3545;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
}
</style>
