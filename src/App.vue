<template>
  <div id="app">

  <b-container class="bv-example-row">
    <div class="row">
      <div class="col-md-3"></div>
      <div class="col-md-6">
        <Header 
          :numCorrect="numCorrect"
          :numTotal="numTotal"
        />
        <QuestionBox 
          v-if="questions.length"
          v-bind:currentQuestion="questions[index]"
          v-bind:next="next"
          :increment="increment"
          :numCorrect="numCorrect"
          :numTotal="numTotal"
        /> 
      </div>
      <div class="col-md-3"></div>
    </div>
  </b-container>
  
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++;
    }, 
    increment(isCorrect){
      if (isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function () {
    let url = 'https://opentdb.com/api.php?amount=10&category=17&type=multiple'
    // https://opentdb.com/api.php?amount=10&category=18&type=multiple
    fetch(url, {
      method: 'get'
    })
    .then( (response) => {
      // console.log(response.json())
      return response.json()
    })
    .then( (jsonData) => {
      this.questions = jsonData.results;
    })
  }
}
</script> 

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #292929;
  margin-top: 60px;
}
</style>
