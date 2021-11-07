<template>
  <div id="app">
    <NavBar />
    <div class="container">
      <router-view class="content"/>
    </div>
  </div>
</template>

<script>
import NavBar from './components/NavBar'
export default {
  components: { NavBar }
}
</script>

<style lang="scss" scoped>
@import "~materialize-css/dist/css/materialize.min.css";
.content {
  padding: 20px 0;
}

</style>
