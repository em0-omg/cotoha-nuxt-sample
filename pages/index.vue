<template>
  <v-container>
    <h2>自然言語処理 デモ ( COTOHA API )</h2>
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
