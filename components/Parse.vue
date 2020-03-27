<template>
  <v-card v-if="token.length > 0">
    <v-container>
      <v-card-text> 2. 構文解析 </v-card-text>
      <v-row>
        <v-col cols="6"
          ><v-textarea
            outlined
            label="INPUT TEXT"
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
        <v-btn @click="getParse">CLICK</v-btn>
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
    this.sentence = "犬は歩く。";
  },
  methods: {
    getParse() {
      if (this.sentence.length === 0) {
        alert("文章を入力してください");
        return;
      }
      const postSentence = this.sentence.replace(/\r?\n/g, "");
      const data = {
        sentence: postSentence
      };
      this.$axios
        .post(`https://api.ce-cotoha.com/api/dev/nlp/v1/parse`, data, {
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
