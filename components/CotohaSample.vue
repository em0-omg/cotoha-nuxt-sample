<template>
  <div>
    <v-card>
      <v-container>
        <v-card-title>
          First Step.
        </v-card-title>
        <v-card-text>Get COTOHA access token.</v-card-text>
        <v-card-actions>
          <v-spacer />
          <template v-if="loading">
            <v-progress-circular
              indeterminate
              color="primary"
            ></v-progress-circular>
          </template>
          <template v-else>
            <v-btn @click="getAccessToken">GET TOKEN</v-btn>
          </template>
        </v-card-actions>
        <v-layout justify-center>
          {{ accessToken }}
        </v-layout>
      </v-container>
    </v-card>
    <br />
    <Sentiment :token="accessToken" />
    <Parse :token="accessToken" />
    <Ne :token="accessToken" />
    <Coreference :token="accessToken" />
    <Keyword :token="accessToken" />
    <Sim :token="accessToken" />
    <SentenceType :token="accessToken" />
    <AttributeVue :token="accessToken" />
    <RemoveFilter :token="accessToken" />
  </div>
</template>
<script>
export default {
  data: () => {
    return {
      accessToken: "",
      loading: false
    };
  },
  components: {
    Sentiment: () => import("~/components/Sentiment"),
    Parse: () => import("~/components/Parse"),
    Ne: () => import("~/components/Ne"),
    Coreference: () => import("~/components/Coreference"),
    Keyword: () => import("~/components/Keyword"),
    Sim: () => import("~/components/Sim"),
    SentenceType: () => import("~/components/SentenceType"),
    AttributeVue: () => import("~/components/Attribute"),
    RemoveFilter: () => import("~/components/RemoveFilter")
  },
  methods: {
    getAccessToken(nippo) {
      this.loading = true;
      const userData = {
        grantType: process.env.GRANT_TYPE,
        clientId: process.env.CLIENT_ID,
        clientSecret: process.env.CLIENT_SECRET
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/v1/oauth/accesstokens`, userData)
        .then(response => {
          this.accessToken = response.data.access_token;
        })
        .catch(e => {
          console.log(e);
        });
      this.loading = false;
    }
  }
};
</script>
