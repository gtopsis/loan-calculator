<template>
  <!-- Modal -->
  <div
    ref="modalRef"
    class="modal fade"
    id="staticBackdrop"
    tabindex="-1"
    aria-labelledby="staticBackdropLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="staticBackdropLabel">Modal title</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          ...
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <button type="button" class="btn btn-primary">Understood</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from "bootstrap";
export default {
  name: "LoanCalcModal",
  props: {
    isShow: { type: Boolean, default: false },
  },
  data() {
    return {
      dialog: null,
    };
  },
  methods: {
    showModal() {
      this.dialog.show();
    },
  },
  mounted() {
    this.dialog = new Modal(this.$refs["modalRef"], { backdrop: true });
    if (this.isShow == true) {
      this.showModal();
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
