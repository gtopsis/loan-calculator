<template>
  <div class="home container-fluid">
    <LoanCalcModal isShow />
  </div>
</template>

<script>
// @ is an alias to /src
import LoanCalcModal from "@/components/LoanCalcModal.vue";

export default {
  name: "Home",
  components: {
    LoanCalcModal,
  },
};
</script>

<style scoped>
.home {
  min-height: 100vh;
  background: rgb(2, 0, 36);
  background: linear-gradient(
    90deg,
    rgba(2, 0, 36, 1) 0%,
    rgba(9, 9, 121, 1) 35%,
    rgba(0, 212, 255, 1) 100%
  );
}
</style>
