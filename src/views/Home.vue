<template>
  <div class="home">
    <router-link class="btn" to="/table">俄罗斯方块</router-link>
    <router-link class="btn" to="/table">贪吃蛇</router-link>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  components: {
  }
}
</script>

<style scoped>
.btn{
  display: block;
}
</style>
