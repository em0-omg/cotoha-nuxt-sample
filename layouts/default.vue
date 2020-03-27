<template>
  <v-app dark>
    <v-container>
      <nuxt />
      <br />
      <br />
      <br />
    </v-container>
    <v-bottom-navigation v-model="bottomNav" fixed="">
      <v-btn value="recent">
        <span>Recent</span>
        <v-icon>mdi-history</v-icon>
      </v-btn>

      <v-btn value="favorites">
        <span>Favorites</span>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn value="nearby">
        <span>Nearby</span>
        <v-icon>mdi-map-marker</v-icon>
      </v-btn>
    </v-bottom-navigation>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      bottomNav: "recent"
    };
  }
};
</script>
