<template>
  <label for="{{radioGroupProps.id}}" class="form-label"
    >{{ radioGroupProps.label }}
    <span v-if="radioGroupProps.required">*</span></label
  >

  <div>
    <div
      v-for="item in radioGroupProps.radios"
      :key="item.value"
      class="form-check form-check-inline"
    >
      <input
        class="form-check-input"
        type="radio"
        id="item.key"
        value="item.value"
      />
      <label class="form-check-label" for="item.key">{{ item.name }}</label>
    </div>
  </div>
</template>

<script >
export default {
  name: "radio-group-vue",
  props: {
    radioGroupProps: {
      radios: Array,
      label: String,
      required: Boolean,
    },
  },
};
</script>

<style scoped>
</style>