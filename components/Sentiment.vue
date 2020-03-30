<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-title>API一覧</v-card-title>
      <v-card-text>1. 感情分析</v-card-text>
      <v-card-subtitle
        >そのテキストの書き手の感情(ネガティブ・ポジティブ)を判定します。</v-card-subtitle
      >
      <v-row>
        <v-col cols="6">
          <v-textarea outlined label="入力" v-model="sentence"></v-textarea>
        </v-col>
        <v-col cols="6">
          {{ result }}
          <br />
          <br />
          <v-divider></v-divider>
          <br />
          <template v-if="result">
            <ul></ul>
          </template>
        </v-col>
      </v-row>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn @click="getSentiment">CLICK</v-btn>
      </v-card-actions>
    </v-container>
  </v-card>
</template>
<script>
export default {
  props: ["token"],
  data: () => {
    return {
      sentence: "",
      result: null
    };
  },
  created() {
    this.sentence = "人生を謳歌しています。";
  },
  methods: {
    getSentiment() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        sentence: postSentence
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/sentiment`, data, {
          headers: {
            Authorization: "Bearer " + this.token
          }
        })
        .then(response => {
          console.log(response.data);
          this.result = response.data.result;
        })
        .catch(e => {
          console.log(e);
          this.result = e;
        });
    }
  }
};
</script>
