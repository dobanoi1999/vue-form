<template>
  <label :for="rangeProps.id" class="form-label">{{ rangeProps.label }}</label>
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
      <div class="flex">
        <span> &lt; </span>
        <span>${{ valueRange }}</span>
        <span> > </span>
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
  height: 0.5rem;
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
  height: 2rem;
}
.range-container .text-range {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: auto;
  display: flex;
  pointer-events: none;
}
.range-container .text-range .flex {
  align-self: center;
  background: green;
  pointer-events: none;
  border-radius: 0.5rem;
  padding: 0.5rem;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  flex-wrap: nowrap;
}

.range-background {
  pointer-events: none;
  height: 0.5rem;
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