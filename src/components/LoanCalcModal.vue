<template>
  <!-- Modal -->
  <div ref="modalRef" class="modal fade" tabindex="-1" aria-hidden="true">
    <div
      class="loanCalcModal modal-dialog modal-fullscreen-md-down modal-lg modal-dialog-centered modal-dialog-scrollable"
    >
      <div class="modal-content">
        <!-- modal header -->
        <div class="modal-header">
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>

        <!-- modal body -->
        <div class="modal-body">
          <div class="container-fluid mb-4 px-2 px-md-5">
            <!-- header -->
            <header class="row mb-3">
              <div class="col-12">
                <h5 class="modal-title text-center">
                  {{ modalTitle }}
                </h5>
              </div>
              <div class="modal-subtitle col-12">
                <p class="text-center">{{ modalSubtitle }}</p>
              </div>
            </header>

            <!-- sliders/ranges -->
            <main class="row mb-3">
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
            </main>

            <!-- monthly installment -->
            <footer
              class="loanCalc__monthly-installment row justify-content-center"
            >
              <div
                class="col-lg-auto px-0 col-sm-12 text-sm-start text-lg-center"
              >
                <strong>
                  <span class="monthly-installment__secondary">
                    {{ monthlyInstallmentLabel }}
                  </span>
                  <span class="monthly-installment__primary"
                    >{{ monthlyInstallment }}€</span
                  >
                </strong>
              </div>
            </footer>
          </div>
        </div>

        <!-- modal footer -->
        <div class="modal-footer p-4">
          <div class="row">
            <div class="col-12 col-md-6">
              <label
                for="telephone"
                class="text-white text-md-start text-sm-center"
              >
                <p class="mb-1">
                  <strong>{{ telephoneInput.label.sentence1 }}</strong>
                </p>
                <p class="mb-sm-2 mb-md-0">
                  <strong>{{ telephoneInput.label.sentence2 }}</strong>
                </p>
              </label>
            </div>

            <!-- form -->
            <div class="col-12 col-md-6">
              <form class="needs-validation" @submit="checkForm">
                <div class="input-group mb-3">
                  <span class="input-group-text" id="basic-addon1"
                    >&#128222;</span
                  >

                  <input
                    type="telephone"
                    id="telephone"
                    class="form-control"
                    :placeholder="telephoneInput.placeholder"
                    v-model="telephoneInput.value"
                    aria-label="Client's telephone"
                    maxlength="10"
                  />

                  <div class="input-group-append">
                    <input
                      class="btn submit-btn text-white"
                      type="submit"
                      :value="formattedSubmitBtnText"
                    />
                  </div>
                </div>

                <!-- error messages -->
                <div v-if="errors.length">
                  <ul class="mb-0 p-0 text-start">
                    <li v-for="(error, index) in errors" :key="index">
                      <span class="error-msg ">{{ error }}</span>
                    </li>
                  </ul>
                </div>

                <!-- success message -->
                <div v-if="isFormValid">
                  <span class="success-msg">
                    {{ successMsg }}
                  </span>
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
        label: "Διάρκεια",
        measurementUnit: " μήνες",
      },
      telephoneInput: {
        label: {
          sentence1: "Συμπλήρωσε εδώ το τηλέφωνό σου.",
          sentence2: "Θα σε καλέσουμε άμεσα.",
        },
        placeholder: "Αριθμός τηλεφώνου...",
        icon: "",
        value: null,
      },
      submitBtnText: "ΚΑΛΕΣΤΕ ΜΕ",
      errors: [],
      isFormValid: false,
      successMsg: "Ευχαριστούμε. Θα σας καλέσουμε άμεσα.",
    };
  },
  computed: {
    monthlyInstallment() {
      let final = (this.finalPrice - this.downPayment) / this.duration;
      return Math.ceil(final);
    },
    formattedSubmitBtnText() {
      return this.submitBtnText.toUpperCase();
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
    checkForm(e) {
      this.errors = [];
      this.isFormValid = false;

      // check userTelephone
      let telephone = this.telephoneInput.value;
      let regex = /^\d{10}$/;

      if (!telephone) {
        this.errors.push("Το πεδίο είναι υποχρεωτικό");
      } else if (!regex.test(telephone)) {
        this.errors.push("Το τηλέφωνο πρέπει να αποτελείται από 10 ψηφία");
      } else {
        this.isFormValid = true;
      }

      e.preventDefault();
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

/* sliders */
.input-group-text {
  background-color: #fff;
  color: #555;
}
.input-group-text + input {
  border-left: none;
}

/* monthly installment */
.monthly-installment__primary {
  font-size: 1.5rem;
}
.monthly-installment__secondry {
  font-size: 0.6rem;
}
.error-msg {
  color: #08b3dc;
}
.success-msg {
  color: yellowgreen;
}
.submit-btn {
  background-color: #55ac36;
}
</style>
