<template>
  <label :for="rangeProps.id" class="form-label">
    {{ rangeProps.label }}
    <span class="text-require" v-if="rangeProps.required">*</span>
  </label>
  <div class="range-container">
    <input
      type="range"
      class="form-range"
      :id="rangeProps.id"
      :min="min"
      :max="max"
      step="500"
      :value="valueRange"
      @input="onRangeChange"
    />
    <div class="range-background">
      <div class="background"></div>
    </div>
    <div class="text-range">
      <div class="text">
        <span>{{ `<    $${valueRange}    >` }}</span>
      </div>
    </div>
  </div>
</template>
    
    <script >
import { onMounted } from "vue";

export default {
  name: "range-vue",
  props: {
    rangeProps: {
      id: String,
      label: String,
      required: Boolean,
    },
    valueRange: String,
  },
  setup(props, context) {
    const min = 1000;
    const max = 10000;
    const setPositionTextValue = (value) => {
      let percent = ((+value - min) / (max - min)) * 100;
      let sliderThumb = document.getElementsByClassName("text-range")[0];
      let background = document.querySelector(".range-background");

      if (sliderThumb && background) {
        sliderThumb.style.left = `${percent}%`;
        sliderThumb.style.transform = `translate(calc(-${percent}%))`;
        // background.style.width = `${percent}%`;
        background.style.background = `linear-gradient(to right, green 0%, green ${percent}%, transparent ${
          percent + 1
        }%, transparent 100% )`;
      }
    };
    const onRangeChange = (e) => {
      setPositionTextValue(e.target.value);
      context.emit("update:valueRange", e.target.value);
    };

    onMounted(() => {
      setPositionTextValue(props.valueRange);
    });
    return {
      min,
      max,
      onRangeChange,
    };
  },
};
</script>
    
<style scoped>
input[type="range"]::-webkit-slider-runnable-track {
  background: white;
  border: 1px solid gray;
  height: 6px;
}
input[type="range"]::-moz-range-thumb {
  -moz-appearance: none;
  appearance: none;
  background: transparent;
}
input[type="range"]::-webkit-slider-thumb {
  -moz-appearance: none;
  appearance: none;
  background: transparent;
  outline: none;
  border: none;
}
input[type="range"]::-moz-range-thumb {
  background: red;
}
.range-container {
  position: relative;
  display: flex;
  justify-content: center;
  height: 25px;
}
.range-container .text-range {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: auto;
  display: flex;
  pointer-events: none;
  background: green;
  border-radius: 4px;
}
.text {
  color: white;
  font-weight: 700;
  font-size: 12px;
  align-self: center;
  padding: 0 12px;
  white-space: pre;
}

.range-background {
  pointer-events: none;
  height: 6px;
  width: 100%;
  /* background: red; */
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  border-radius: 0.2rem;
}
.form-range {
  height: 100%;
  padding: 0 3px;
}
</style>