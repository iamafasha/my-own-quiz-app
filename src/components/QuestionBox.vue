<template>
    <div class="question-box-container">
    <b-jumbotron >
        <template slot:lead>
             {{ currentQuestion.question }} 
        </template>

        <hr class="my-4">

        <b-list-group>
        <b-list-group-item  
            v-for="(answer , index) in shuffledAnswers"  
            :key="index"
            @click="selectedAnswer(index)"
            :class="answerClass(index)"
             >
             {{ answer }}
        </b-list-group-item>
         
        </b-list-group>

        <b-button
        @click="submitAnswer"
        :disabled="selectedIndex==null || answered"

        >Submit</b-button>
        <b-button @click="next" variant="success" href="#">Next </b-button>
    </b-jumbotron>
    </div>
</template>



<script>

import  _ from 'lodash'
export default {
    props:{
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex:null,
            correctIndex:null,
            shuffledAnswers:null,
            answered:false
        }
    },
    methods: {
      selectedAnswer(index){
          this.selectedIndex=index
          console.log(index)
      },
      shuffleAnswers(){
          let answers =[...this.currentQuestion.incorrect_answers , this.currentQuestion.correct_answer]
          this.shuffledAnswers = _.shuffle(answers)
          this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer(){
          let isCorrect =false
        if( this.selectedIndex == this.correctIndex ){
        
        isCorrect=true
        }

          this.answered=true
          this.increment(isCorrect);

      },
      answerClass(index){
        let answerClass=""
        let answered=this.answered
        let correctIndex=this.correctIndex
        let selectedIndex=this.selectedIndex
        if(!answered &&  selectedIndex === index){
            answerClass="selected"
        }else if( answered && correctIndex === index){
            answerClass="correct"
        } else if( answered &&  selectedIndex==index &&  correctIndex !== index){
            answerClass="incorect"
        }

        return answerClass
      }
    },
    watch: {
        currentQuestion:{
            immediate:true,
            handler(){
                this.selectedIndex =null
                this.answered=false
                this.shuffleAnswers()
            }
        }
    },
    computed: {
        answers(){
         let answers =[...this.currentQuestion.incorrect_answers ]
         answers.push(this.currentQuestion.correct_answer)
         return answers;
        }
    }
}
</script>


<style scoped>
    .list-group{
        margin-bottom:15px ;
    }

     .list-group-item:hover{
        background:#eeeeee;
        cursor: pointer;
    }

    .list-group-item.selected{
        background: lightblue; 
    }

    .list-group-item.correct{
        background: lightgreen; 
    }

    .list-group-item.incorect{
        background: red; 
    }

    .btn{
        margin: 0 5px;
    }
</style>