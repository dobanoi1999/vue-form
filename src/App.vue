<template>
  <div class="app">
    <div class="container-fluid g-0">
      <div class="row g-0">
        <div class="col-4">
          <img
            alt="background"
            class="img-background"
            src="./assets/img/bg.jpeg"
          />
        </div>
        <div class="col-8 p-4"><Form /></div>
      </div>
    </div>
  </div>
</template>

<script>
import Form from "./components/formVue.vue";
import "./assets/css/global.css";
import "./assets/css/resetcss.css";

export default {
  name: "App",
  components: { Form },
};
</script>

<style scoped>
.img-background {
  width: auto;
  height: 100vh;
  object-fit: cover;
}
</style>

