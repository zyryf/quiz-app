<template>
  <div id="app">
    <Header :score="score" />

    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :CurrentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      score: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.score++;
      }
    }
  },
  mounted: function() {
    fetch(
      "https://opentdb.com/api.php?amount=10&difficulty=medium&type=multiple",
      { method: "get" }
    )
      .then(response => {
        return response.json();
      })
      .then(data => {
        this.questions = data.results;
      })
      .catch(err => {
        alert(err);
      });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
