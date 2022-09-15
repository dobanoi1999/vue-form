<template>
  <label for="{{selectProps.id}}" class="form-label"
    >{{ selectProps.label }} <span v-if="selectProps.required">*</span></label
  >
  <select class="form-select" id="{{selectProps.id}}">
    <option
      v-for="option in selectProps.options"
      :key="option.key"
      value="option.value"
    >
      {{ option.name }}
    </option>
  </select>
</template>
  
  <script >
export default {
  name: "select-vue",
  props: {
    selectProps: {
      id: String,
      label: String,
      required: Boolean,

      options: Array,
    },
  },
};
</script>
  
  <style scoped>
.form-label span {
  color: red;
}
</style>