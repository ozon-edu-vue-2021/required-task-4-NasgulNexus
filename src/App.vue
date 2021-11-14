<template>
  <div id="app">
    <FormValidation />
  </div>
</template>

<script>
import FormValidation from "./components/FormValidation.vue";

export default {
  name: "App",
  components: {
    FormValidation
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

* {
  box-sizing: border-box;
}
</style>
