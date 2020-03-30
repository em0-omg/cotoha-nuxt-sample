<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text>3. 固有表現抽出</v-card-text>
      <v-card-subtitle
        >人名や地名、日付表現(時間、日付)、組織名、量的表現(金額、割合)、人工物の8種類の固有表現と、200種類以上のクラス数を持つ拡張固有表現を出力します。</v-card-subtitle
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
        <v-btn @click="getNe">CLICK</v-btn>
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
    this.sentence = "昨日は東京駅を利用した。";
  },
  methods: {
    getNe() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        sentence: postSentence
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/ne`, data, {
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
