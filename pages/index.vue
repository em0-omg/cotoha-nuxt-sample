<template>
  <v-container>
    <h2>COTOHA SAMPLE</h2>
    <br />
    <CotohaSample />
  </v-container>
</template>

<script>
export default {
  components: {
    CotohaSample: () => import("~/components/CotohaSample")
  }
};
</script>
