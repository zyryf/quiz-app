<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{CurrentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        :disabled="selectedIndex===null || answered"
        class="mx-1"
        variant="primary"
        @click="checkAnswer"
      >Submit</b-button>
      <b-button :disabled="!answered" class="mx-1" @click="next" variant="success">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    CurrentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.CurrentQuestion.incorrect_answers,
        this.CurrentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
    },
    checkAnswer() {
      let isCorrect = false;
      const correctAnswer = this.CurrentQuestion.correct_answer;
      const userAnswer = this.shuffledAnswers[this.selectedIndex];
      if (userAnswer === correctAnswer) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.CurrentQuestion.correct_answer
      );
    },
    answerClass(index) {
      let classType;
      if (!this.answered && this.selectedIndex === index) {
        classType = "selected";
      } else if (this.answered && this.correctIndex === index) {
        classType = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        classType = "incorrect";
      }

      return classType;
    }
  },
  mounted() {
    this.shuffleAnswers();
  },
  watch: {
    CurrentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();
      this.answered = false;
      this.correctIndex = null;
    }
  },

  computed: {
    answers() {
      let answers = [...this.CurrentQuestion.incorrect_answers];
      answers.push(this.CurrentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin: 20px;
}

.list-group-item:hover {
  background-color: #ddd;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightcoral;
}
</style>