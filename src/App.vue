<template>
  <p>
    <router-link class="btn btn-primary" to="/">Главная</router-link> |
    <router-link class="btn btn-primary" to="/new">Новое резюме</router-link>
  </p>
  <router-view></router-view>
  <div class="container-xl ">
    <!-- <FormComponent/> -->
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {}
}
</script>

<style>

</style>