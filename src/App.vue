<template>
  <div id="app">

    <b-container class="bv-example-row">
  <b-row>
    <b-col sm=6 offset=3>
      <Header
        :numCorrect="numCorrect"
        :numTotal="numTotal"
        :questions_no="questions.length"
        :question_counted="question_counter"
       />
      <QuestionBox 
        v-if="questions.length && index!==false"
            :currentQuestion="questions[index]" 
            :next="next"
            :increment="increment"
      
       />
    <ScoreBoard 
       v-if="index===false"
       :numCorrect="numCorrect"
       :numTotal="numTotal"
     />
  
    </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>

import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import ScoreBoard from './components/ScoreBoard'

export default {
  name: 'app',
  data(){
    return {
      questions:[],
      index:0,
      numCorrect:0,
      numTotal:0
    }
  },
  components: {
    Header,
    QuestionBox,
    ScoreBoard
  },
  mounted: function () {
    fetch('http://localhost/quiz.php',{
      method:'get'
    }).then((response)=>{
      return (response.json())
    }).then((jsonData)=>{
        this.questions= jsonData.results
    })
  },
  methods: {
    getQuestionsFromDb(){
      fetch('http://localhost/quiz.php',{
      method:'get'
    }).then((response)=>{
      return (response.json())
    }).then((jsonData)=>{
        this.questions= jsonData.results
    })
    },
    next(){
     if(this.index<this.questions.length-1){
        ++this.index
     }else{
       this.index=false
     }
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }

  },
    computed: {
        question_counter(){
            return this.index+1;
        }
    },
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
