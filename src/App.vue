<template>
  <div class="app">
    <div class="card">
      <div class="card-body">
        <div class="container-fluid">
          <div class="row g-0">
            <div class="col-4"><img src="./assets/img/bg.jpeg" /></div>
            <div class="col-8 p-4"><Form /></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Form from "./components/form.vue";

export default {
  name: "App",
  components: { Form },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}

img {
  width: auto;
  height: 100vh;
  object-fit: cover;
}
</style>

