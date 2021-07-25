<template>
  <!-- Range -->
  <div class="container-fluid px-0">
    <div class="row justify-content-between">
      <div class="col-auto ps-0">
        <strong>
          <label for="disabledRange" class="form-label mb-0">{{ label }}</label>
        </strong>
      </div>
      <div class="col-auto pe-0 text-right">
        <h4 class="mb-0">{{ formattedCurrValue }}{{ measurementUnit }}</h4>
      </div>
    </div>
    <div class="row">
      <input
        type="range"
        class="form-range"
        v-model="currValue"
        :min="min"
        :max="max"
        :step="step"
        @change="emitValue"
      />
    </div>
    <div class="row justify-content-between">
      <div class="col-auto ps-0">
        <span class="secondary text-left"
          >{{ formattedMinValue }}{{ measurementUnit }}</span
        >
      </div>
      <div class="col-auto pe-0">
        <span class="secondary text-right"
          >{{ formattedMaxValue }}{{ measurementUnit }}</span
        >
      </div>
    </div>
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
.form-label {
  vertical-align: middle;
}
</style>
