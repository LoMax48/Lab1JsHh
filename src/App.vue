<template>
  <div class="app">
    <NavbarComponent />
    <FormComponent />
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent";
import FormComponent from "@/components/FormComponent";

export default {
  name: 'App',
  components: {
    NavbarComponent,
    FormComponent,
  }
}
</script>

<style>

</style>