<template>
  <div class="container">
    <Header
      :numCorrect="numCorrect"
      :totalQues="totalQues"
    ></Header>
    <QuestionBox
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
    ></QuestionBox>
  </div>
</template>

<script>
import QuestionBox from "./components/QuestionBox.vue";
import Header from "./components/Header.vue";

export default {
  name: "app",
  components: {
    QuestionBox,
    Header
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      totalQues: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.totalQues++
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=18&type=multiple", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
