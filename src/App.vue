<template>
  <div>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/main">Main</router-link>
      <router-link to="/duel">Duel</router-link>
    </nav>

    <router-view></router-view>
  </div>
</template>

<style scoped>
nav {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
}
</style>

<script>
export default {
  name: 'App',
}
</script>
