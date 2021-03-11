<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          v-bind:key="index"
          @click.prevent="selectedAnswer(index)"
          :disabled="answered"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  watch: {
    currQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectedAnswer: function (index) {
      this.selectedIndex = index;
    },
    shuffleAnswers: function () {
      let answers = [
        ...this.currQuestion.incorrect_answers,
        this.currQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currQuestion.correct_answer
      );
    },
    answerClass: function(index){
        let answerCls = '';

        if(!this.answered && this.selectedIndex === index){
          answerCls = 'selected';
        }else if(this.answered && this.correctIndex === index){
          answerCls = 'correct';
        } else if(this.answered && this.selectedIndex === index && this.correctIndex !== index) {
          answerCls = 'incorrect'
        }

        return answerCls;

    },
    submitAnswer: function () {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.increment(isCorrect);
      this.answered = true;
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lemonchiffon;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>
