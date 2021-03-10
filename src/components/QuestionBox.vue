<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          v-bind:key="index"
          @click.prevent="selectedAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currQuestion: Object,
    next: Function,
  },
  data(){
    return {
      selectedIndex:null
    }
  },
  computed: {
    answers: function () {
      let answers = [...this.currQuestion.incorrect_answers];
      answers.push(this.currQuestion.correct_answer);
      return answers;
    },
  },
  methods: {
    selectedAnswer: function(index){
      this.selectedIndex = index;
    }
  }
};
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
}

.list-group-item:hover{
  background: #EEE;
  cursor: pointer;
}

.btn{
  margin:0 5px;
}

.selected{
  background-color: lemonchiffon;
}

.correct{
  background-color: green;
}

.incorrect{
  background-color: red;
}
</style>
