<template>
  <app-form title="Форма создания клиента" />
</template>

<script>
import AppForm from "./components/AppForm.vue";

export default {
  name: "App",

  components: {
    AppForm,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@300;600&display=swap");
#app {
  font-family: "Nunito", sans-serif;
  text-align: center;
  box-sizing: border-box;
}
</style>
