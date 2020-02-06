<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group class="mb-2" v-for="(answer, index) in answers" :key="index">
        <b-list-group-item
          class="mb-10"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected': '']"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" class="mr-4" @click="submitAnswer">Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
    };
  },
  mounted() {
    console.log(this.currentQuestion);
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex == this.correctIndex) {
        isCorrect = true;
      }
      return isCorrect;
    }
  }
};
</script>

<style scoped>
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}
.selected {
  background-color: lightblue;
}

.correct {
  background-color: green;
}

.incorrect {
  background-color: red;
}
</style>