<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group
        class="mb-2"
        v-for="(answer, index) in shuffledAnswers"
        :key="index"
      >
        <b-list-group-item
          class="mb-10"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        class="mr-4"
        @click="submitAnswer"
        :disabled="selectedIndex == null || answered"
      >
        Submit
      </b-button>
      <b-button
        variant="success"
        @click="next"
      >
        Next
      </b-button>
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
      correctIndex: null,
      answered: false,
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
        this.answered = false;
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
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      )
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex == this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      return isCorrect;
    },
    answerClass(index) {
      let answerClass = '';

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (
          this.answered &&
          this.selectedIndex === index &&
          this.correctIndex !== index
        ) {
        answerClass = 'incorrect'
      }

      return answerClass;
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