<template>
  <label for="{{inputProps.id}}" class="form-label"
    >{{ inputProps.label }} <span v-if="inputProps.required">*</span></label
  >
  <input
    type="{{inputProps.type}}"
    class="form-control"
    id="{{inputProps.id}}"
  />
</template>

<script >
export default {
  name: "input-vue",
  props: {
    inputProps: {
      id: String,
      label: String,
      required: Boolean,
      type: String,
    },
  },
};
</script>

<style scoped>
.form-label span {
  color: red;
}
</style>