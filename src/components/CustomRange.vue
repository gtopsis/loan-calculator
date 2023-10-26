<template>
  <!-- Range -->
  <div class="range-container container-fluid px-0">
    <header class="row justify-content-between">
      <div class="col-auto ps-0 mb-2">
        <strong>
          <label for="range" class="range-container__label mb-0">{{
            label
          }}</label>
        </strong>
      </div>
      <div class="col-auto pe-0 text-right">
        <strong class="range-container__value mb-0">
          <span class="range-value__primary"> {{ formattedCurrValue }} </span>
          <span class="range-value__secondary">
            {{ measurementUnit }}
          </span>
        </strong>
      </div>
    </header>

    <main class="row mb-2">
      <input
        type="range"
        class="range-container__slider"
        id="range"
        v-model="currValue"
        :min="min"
        :max="max"
        :step="step"
        @change="emitValue"
      />
    </main>

    <footer class="row justify-content-between">
      <div class="col-auto ps-0">
        <span class="range-container__min-value text-left"
          >{{ formattedMinValue }}{{ measurementUnit }}</span
        >
      </div>
      <div class="col-auto pe-0">
        <span class="range-container__max-value text-right"
          >{{ formattedMaxValue }}{{ measurementUnit }}</span
        >
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "CustomRange",
  props: {
    min: { type: Number, default: 0 },
    max: { type: Number, default: 0 },
    step: { type: Number, default: 1 },
    initValue: { type: Number, default: 0 },
    label: { type: String, default: "" },
    measurementUnit: { type: String, default: "" },
  },
  data() {
    return {
      currValue: 0,
    };
  },
  computed: {
    formattedCurrValue() {
      let num = this.currValue;
      return this.formatNummericValue(num);
    },
    formattedMinValue() {
      let num = this.min;
      return this.formatNummericValue(num);
    },
    formattedMaxValue() {
      let num = this.max;
      return this.formatNummericValue(num);
    },
  },
  methods: {
    formatNummericValue(num) {
      const seperator = ".";
      return num
        .toString()
        .replace(/(\d)(?=(\d{3})+(?!\d))/g, `$1${seperator}`);
    },
    emitValue(event) {
      this.$emit("change", event.target.value);
    },
  },
  mounted() {
    this.currValue = this.initValue;
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-header,
.modal-footer {
  border: none;
}

.range-container__label {
  font-size: 0.9rem;
}
.range-container__value {
  font-weight: bold;
}
.range-container__value .range-value__primary {
  font-size: 1.25rem;
}

.range-container__value .range-value__secondary {
  font-size: 1rem;
}

.range-container__min-value,
.range-container__max-value {
  font-size: 0.7rem;
  color: grey;
}

input[type="range"]::-webkit-slider-thumb,
input[type="range"]::-moz-range-thumb {
  cursor: pointer;
}
</style>
