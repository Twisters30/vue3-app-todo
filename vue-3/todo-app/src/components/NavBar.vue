<template>
  <nav>
    <div class="nav-wrapper">
     <div class="container">
       <a href="#" class="brand-logo">Task manager</a>
       <ul id="nav-mobile" class="nav-mobile__list right">
         <router-link tag="li" active-class="active-route" to="/Create"><a href="#">Create task</a></router-link>
         <router-link tag="li" active-class="active-route" to="/viewTask"><a href="#">View tasks</a></router-link>
       </ul>
     </div>
    </div>
  </nav>
</template>

<script>
import 'materialize-css/dist/js/materialize.min'
export default {
  name: 'NavBar',
  mounted () {
    this.$router.push({ path: 'viewTask' })
  }
}
</script>

<style lang="scss" scoped>
  .nav-mobile {
    &__list {
      display: flex;
    }
  }
  .active-route {
    opacity: .7;
    background-color: #ef5350;
  }
</style>
