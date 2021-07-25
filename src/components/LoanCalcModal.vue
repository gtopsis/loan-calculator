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
      class="modal-dialog modal-fullscreen-md-down modal-lg modal-dialog-centered modal-dialog-scrollable"
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
                  @change="updateValueA"
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
                  @change="updateValueB"
                  :measurementUnit="sliderB.measurementUnit"
                ></custom-range>
              </div>
            </div>

            <div class="row justify-content-center">
              <div class="col-auto">
                <strong>
                  <span>
                    {{ monthlyInstallmentLabel }}
                  </span>
                  <span>{{ monthlyInstallment }}€</span>
                </strong>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <div class="row">
            <div class="col-6">
              <label for=""></label>
            </div>
            <div class="col-6">
              <form action="" method="post">
                <div class="row">
                  <div class="col-auto">
                    <div class="input-group">
                      <span class="input-group-text" id="inputGroupPrepend2"
                        >@</span
                      >
                      <input
                        type="telephone"
                        class="form-control"
                        id="validationDefaultUsername"
                        aria-describedby="inputGroupPrepend2"
                        required
                      />
                    </div>
                  </div>
                  <div class="col-md-auto">
                    <button type="button" class="btn btn-primary">
                      Understood
                    </button>
                  </div>
                </div>
              </form>
            </div>
          </div>
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
      monthlyInstallmentLabel: "Μηνιαία Δόση:",
      dialog: null,
      finalPrice: 20000,
      downPayment: 200,
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
    monthlyInstallment() {
      let final = (this.finalPrice - this.downPayment) / this.duration;
      return Math.ceil(final);
    },
  },
  methods: {
    updateValueA(v) {
      this.downPayment = v;
    },
    updateValueB(v) {
      this.duration = v;
    },
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

.modal-footer {
  background-color: #223657;
}

.modal-title {
  font-weight: bold;
  font-size: 1.2rem;
}

.modal-subtitle {
  font-size: 0.8rem;
}
</style>
