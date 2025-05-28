<template>
  <div class="home">
    <h1>Welcome to IE Bank</h1>
    <router-link to="/accounts">
      <button>Go to Accounts</button>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.home {
  text-align: center;
  margin-top: 50px;
}
button {
  padding: 10px 20px;
  font-size: 18px;
}
</style>
