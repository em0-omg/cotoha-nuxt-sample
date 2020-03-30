<template>
  <div>
    <v-card>
      <v-container>
        <v-card-title>最初に</v-card-title>
        <v-card-subtitle>ワンタイムトークンなので他の誰かがトークンを取得しなおすとエラーが出ます。画面を更新するとトークンを再発行します。</v-card-subtitle>
        <v-card-subtitle>また無料アカウントのため1日100リクエスト程で停止してしまいます。</v-card-subtitle>

        <!--
        <v-card-actions>
          <v-spacer />
          <template v-if="loading">
            <v-progress-circular indeterminate color="primary"></v-progress-circular>
          </template>
          <template v-else>
            <v-btn @click="getAccessToken">GET TOKEN</v-btn>
          </template>
        </v-card-actions>
        -->
        <v-layout justify-center>{{ accessToken }}</v-layout>
      </v-container>
    </v-card>
    <br />
    <Sentiment :token="accessToken" id="sentiment" />
    <Parse :token="accessToken" id="parse" />
    <Ne :token="accessToken" id="ne" />
    <Coreference :token="accessToken" id="coreference" />
    <Keyword :token="accessToken" id="keyword" />
    <Sim :token="accessToken" id="sim" />
    <SentenceType :token="accessToken" id="sentencetype" />
    <AttributeVue :token="accessToken" id="attr" />
    <RemoveFilter :token="accessToken" id="removefilter" />
    <Summary :token="accessToken" id="summary" />
    <v-bottom-navigation v-model="bottomNav" fixed>
      <v-btn value="sentiment" v-scroll-to="'#sentiment'">
        <span>感情分析</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="parse" v-scroll-to="'#parse'">
        <span>構文解析</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="ne" v-scroll-to="'#ne'">
        <span>固有表現</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="coreference" v-scroll-to="'#coreference'">
        <span>照応解析</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="keyword" v-scroll-to="'#keyword'">
        <span>キーワード</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="sim" v-scroll-to="'#sim'">
        <span>類似度</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="sentencetype" v-scroll-to="'#sentencetype'">
        <span>文タイプ</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="attibute" v-scroll-to="'#attr'">
        <span>ユーザ属性</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="removefilter" v-scroll-to="'#removefilter'">
        <span>淀み除去</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>

      <v-btn value="summary" v-scroll-to="'#summary'">
        <span>要約</span>
        <v-icon>mdi-check-circle</v-icon>
      </v-btn>
    </v-bottom-navigation>
  </div>
</template>
<script>
export default {
  data: () => {
    return {
      bottomNav: "sentiment",
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
    RemoveFilter: () => import("~/components/RemoveFilter"),
    Summary: () => import("~/components/Summary")
  },
  mounted() {
    console.log("mounted");
    this.getAccessToken();
    console.log("token get", this.accessToken);
  },
  methods: {
    getAccessToken() {
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
          this.result = e;
        });
      this.loading = false;
    }
  }
};
</script>
