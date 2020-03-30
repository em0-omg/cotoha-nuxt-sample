<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text>10. 要約</v-card-text>
      <v-card-subtitle>要約文を返します。</v-card-subtitle>
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
        <v-btn @click="getSummary">CLICK</v-btn>
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
    this.sentence =
      "前線が太平洋上に停滞しています。一方、高気圧が千島近海にあって、北日本から東日本をゆるやかに覆っています。関東地方は、晴れ時々曇り、ところにより雨となっています。東京は、湿った空気や前線の影響により、晴れ後曇りで、夜は雨となるでしょう。";
  },
  methods: {
    getSummary() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        document: postSentence,
        mode: "extract",
        sent_len: "1",
        lang: "ja"
      };
      console.log(data);
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/beta/summary`, data, {
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
