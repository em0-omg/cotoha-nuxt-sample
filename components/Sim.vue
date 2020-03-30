<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text>6. 類似度算出</v-card-text>
      <v-card-subtitle
        >テキスト間の意味的な類似度を算出・出力します。</v-card-subtitle
      >
      <v-row>
        <v-col cols="6">
          <v-textarea outlined label="入力1" v-model="sentence1"></v-textarea>
        </v-col>
        <v-col cols="6">
          <v-textarea outlined label="入力2" v-model="sentence2"></v-textarea>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
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
        <v-btn @click="getSim">CLICK</v-btn>
      </v-card-actions>
    </v-container>
  </v-card>
</template>
<script>
export default {
  props: ["token"],
  data: () => {
    return {
      sentence1: "",
      sentence2: "",
      result: null
    };
  },
  created() {
    this.sentence1 = "近くのレストランはどこですか？";
    this.sentence2 = "このあたりの定食屋はどこにありますか？";
  },
  methods: {
    getSim() {
      if (this.sentence1.length === 0 || this.sentence2.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence1 = this.sentence1.replace(/\r?\n/g, "");
      const postSentence2 = this.sentence2.replace(/\r?\n/g, "");
      const data = {
        s1: postSentence1,
        s2: postSentence2
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/similarity`, data, {
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
