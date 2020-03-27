<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text> 7. 文章タイプ </v-card-text>
      <v-card-subtitle
        >*入力として日本語で記述された文を受け取り、文の法(叙述/疑問/命令)タイプと発話行為タイプを判定・出力します。
      </v-card-subtitle>
      <v-row>
        <v-col cols="6"
          ><v-textarea
            outlined
            label="INPUT 1 TEXT"
            v-model="sentence"
          ></v-textarea
        ></v-col>
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
        <v-btn @click="getType">CLICK</v-btn>
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
    this.sentence = "あなたの名前は何ですか？";
  },
  methods: {
    getType() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        sentence: postSentence
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/sentence_type`, data, {
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
        });
    }
  }
};
</script>
