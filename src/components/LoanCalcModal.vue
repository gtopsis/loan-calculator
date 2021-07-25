<template>
  <!-- Modal -->
  <div
    ref="modalRef"
    class="modal fade"
    tabindex="-1"
    aria-labelledby="staticBackdropLabel"
    aria-hidden="true"
  >
    <div
      class="modal-dialog  modal-lg modal-dialog-centered modal-dialog-scrollable"
    >
      <div class="modal-content">
        <div class="modal-header">
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body px-sm-2 px-md-5">
          <div class="container-fluid">
            <div class="row mb-4">
              <div class="col-12">
                <h5 class="modal-title text-center">
                  {{ modalTitle }}
                </h5>
              </div>
              <div class="modal-subtitle col-12">
                <p class="text-center">{{ modalSubtitle }}</p>
              </div>
            </div>

            <!-- sliders -->
            <div class="row mb-4">
              <div class="col-12 mb-4">
                <custom-range
                  :min="sliderA.min"
                  :max="sliderA.max"
                  :label="sliderA.label"
                  :step="sliderA.step"
                  :initValue="sliderA.initValue"
                  :measurementUnit="sliderA.measurementUnit"
                ></custom-range>
              </div>
              <div class="col-12">
                <custom-range
                  :min="sliderB.min"
                  :max="sliderB.max"
                  :label="sliderB.label"
                  :step="sliderB.step"
                  :initValue="sliderB.initValue"
                  :measurementUnit="sliderB.measurementUnit"
                ></custom-range>
              </div>
            </div>

            <div class="row justify-content-center">
              <div class="col-auto">
                <strong>
                  Μηνιαία Δόση: <span>{{ total }}€</span>
                </strong>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary">Understood</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from "bootstrap";
import CustomRange from "../components/CustomRange.vue";

export default {
  name: "LoanCalcModal",
  components: {
    CustomRange,
  },
  props: {
    isShow: { type: Boolean, default: false },
  },
  data() {
    return {
      modalTitle: "Υπολογισμός Δόσης",
      modalSubtitle: "Επίλεξε την προκαταβολή και την διαρκεια που σε συμφέρει",
      dialog: null,
      finalPrice: 2000,
      downsidePayment: 200,
      duration: 12,
      sliderA: {
        min: 0,
        step: 50,
        max: 10700,
        initValue: 4800,
        label: "Προκαταβολή",
        measurementUnit: "€",
      },
      sliderB: {
        min: 0,
        max: 72,
        step: 1,
        initValue: 72,
        label: "Δόση",
        measurementUnit: " μήνες",
      },
    };
  },
  computed: {
    total() {
      return (this.finalPrice - this.downsidePayment) / this.duration;
    },
  },
  methods: {
    showModal() {
      this.dialog.show();
    },
  },
  mounted() {
    let refModal = this.$refs["modalRef"];
    this.dialog = new Modal(refModal, { backdrop: true });
    if (this.isShow === true) {
      this.showModal();
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-header,
.modal-footer {
  border: none;
}

.modal-title {
  font-weight: bold;
  font-size: 1.2rem;
}

.modal-subtitle {
  font-size: 0.8rem;
}
</style>
