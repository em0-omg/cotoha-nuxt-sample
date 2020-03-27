<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text> 9. 言い淀み除去 </v-card-text>
      <v-card-subtitle></v-card-subtitle>
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
        <v-btn @click="getFilter">CLICK</v-btn>
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
      "えーーっと、あの、今日の打ち合わせでしたっけ。すみません、ちょっと、急用が入ってしまって。";
  },
  methods: {
    getFilter() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        text: postSentence
      };
      this.$axios
        .post(
          `https://api.ce-cotoha.com/api/dev/nlp/beta/remove_filler`,
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
        });
    }
  }
};
</script>
