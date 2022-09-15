<template>
  <label class="form-label"
    >{{ radioGroupProps.label }}
    <span class="text-require" v-if="radioGroupProps.required">*</span></label
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
        :id="item.value"
        :value="item.value"
        :checked="item.value === valueRadio"
        @input="$emit('update:valueRadio', $event.target.value)"
      />
      <label class="form-check-label" :for="item.value">{{ item.name }}</label>
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
    valueRadio: {
      type: String,
    },
  },
};
</script>

<style scoped>
</style>