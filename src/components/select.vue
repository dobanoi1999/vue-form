<template>
  <div>
    <label for="{{selectProps.id}}" class="form-label">
      {{ selectProps.label }}
      <span class="text-require" v-if="selectProps.required">*</span>
    </label>

    <select
      class="form-select form-select-lg"
      @input="$emit('update:selectValue', $event)"
      id="{{selectProps.id}}"
      :value="selectValue"
    >
      <option
        v-for="option in selectProps.options"
        :key="option.key"
        :value="option.value"
      >
        {{ option.name }}
      </option>
    </select>
  </div>
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
    selectValue: String,
  },
};
</script>
  
