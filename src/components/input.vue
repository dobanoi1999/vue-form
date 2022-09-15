
<template>
  <label :for="inputProps.id" class="form-label"
    >{{ inputProps.label }}
    <span class="text-require" v-if="inputProps.required">*</span></label
  >
  <input
    :type="inputProps.type"
    class="form-control"
    :id="inputProps.id"
    :value="valueInput"
    @input="$emit('update:valueInput', $event.target.value)"
    v-bind="$attrs"
  />
  <div>
    <span class="error-message" :class="errorMessage ? 'show-error' : ''">{{
      errorMessage
    }}</span>
  </div>
</template>

<script >
export default {
  name: "input-vue",
  props: {
    inputProps: {
      type: Object,
      default() {
        return { type: "text" };
      },
    },
    valueInput: {
      type: String,
    },
    errorMessage: {
      type: String,
    },
  },
};
</script>

<style scoped>
</style>