<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text>4. 照応解析</v-card-text>
      <v-card-subtitle
        >テキスト中の「そこ」「それ」などの指示詞や「彼」「彼女」などの代名詞と対応する先行詞を抽出</v-card-subtitle
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
        <v-btn @click="getCor">CLICK</v-btn>
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
    this.sentence = "太郎は友人です。彼は焼き肉を食べた。";
  },
  methods: {
    getCor() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        document: postSentence
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/coreference`, data, {
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
