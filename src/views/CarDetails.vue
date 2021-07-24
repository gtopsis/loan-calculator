<template>
  <div class="home container-fluid">
    <img alt="Vue logo" src="../assets/spotawheel-logo.png" />
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
  background-color: #b8c6db;
  background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 74%);
}
</style>
