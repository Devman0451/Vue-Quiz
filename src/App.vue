<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :done="done"
           />
          <b-button 
            @click="restartQuiz" 
            variant="danger"
            :disabled="!done"
          >
          Start A New Quiz
        </b-button>
        </b-col>
      </b-row>
</b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      done: false
    }
  },
  methods: {
    next() {
      if(this.index >= this.questions.length - 1) {
        this.done = true;
      } else {
        this.index++
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      } 
      this.numTotal++
    },
    fetchData() {
      fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
        method: 'get'
      })
      .then(res => res.json())
      .then(data => {
        this.questions = data.results
      })
    },
    restartQuiz() {
      this.index = 0;
      this.numCorrect = 0
      this.numTotal = 0
      this.done = false
      this.fetchData()
    }
  },
  mounted: function() {
    this.fetchData();
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
