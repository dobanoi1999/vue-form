<template>
  <label for="{{rangeProps.id}}" class="form-label">{{
    rangeProps.label
  }}</label>
  <input type="range" class="form-range" id="{{rangeProps.id}}" />
</template>
    
    <script >
export default {
  name: "range-vue",
  props: {
    rangeProps: {
      id: String,
      label: String,
      required: Boolean,
    },
  },
};
</script>
    
<style scoped>
input[type="range"]::-webkit-slider-runnable-track {
  background: red;
}

.form-label span {
  color: red;
}
</style>