<template>
  <div class="d-flex flex-column">
    <label class="form-label">
      {{ radioGroupProps.label }}
      <span class="text-require" v-if="radioGroupProps.required">*</span>
    </label>

    <div class="flex-grow-1 form-content">
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
          @input="$emit('update:valueRadio', $event)"
        />
        <label class="form-check-label" :for="item.value">
          {{ item.name }}
        </label>
      </div>
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
.form-content {
  display: flex;
  align-items: center;
}
.form-check-input:checked {
  background-color: green;
  border-color: green;
}
.form-check-input:focus {
  border-color: #459953;
  outline: 0;
  box-shadow: 0 0 0 0.25rem rgba(13, 253, 105, 0.25);
}
.form-check-label {
  font-weight: 500;
}
</style>