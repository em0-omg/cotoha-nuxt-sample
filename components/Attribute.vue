<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text>8. ユーザ属性推定</v-card-text>
      <v-card-subtitle
        >年代、性別、趣味、職業などの人物に関する属性を推定・出力します。</v-card-subtitle
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
        <v-btn @click="getAttr">CLICK</v-btn>
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
    this.sentence = "私は昨日田町駅で飲みに行ったら奥さんに怒られた。";
  },
  methods: {
    getAttr() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        document: postSentence
      };
      this.$axios
        .post(
          `https://api.ce-cotoha.com/api/dev/nlp/beta/user_attribute`,
          data,
          {
            headers: {
              Authorization: "Bearer " + this.token
            }
          }
        )
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
