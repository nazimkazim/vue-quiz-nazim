<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group class="pb-3">
        <b-list-group-item
          button
          v-for="(answer, index) in shuffledAnswers"
          :key="answer"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button variant="primary" href="#" v-on:click="submitAnswer" :disabled="selectedIndex === null || clicked"
        >Submit</b-button
      >
      <b-button v-on:click="nextQuestion" variant="success" href="#" 
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    currentAnswers: Array,
    increment:Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex:null,
      shuffledAnswers: [],
      clicked:false
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.clicked = false
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = this.currentAnswers;
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.clicked=true
      this.increment(isCorrect)
    }
  }
};
</script>

<style scoped>
.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightred;
}
</style>
