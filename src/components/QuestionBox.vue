<template>
<div class = "question-box-container">
 <b-jumbotron>

    <template v-slot:lead>
      <b>Q{{questionIndex+1}}: </b>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group >
      <b-list-group-item v-for="(answer, index) in shuffledAnswers" 
         :key="index"
         @click.prevent="selectAnswer(index)"
         :class="answerClass(index)">
         {{answer}}
      </b-list-group-item>
    </b-list-group>
  
  </b-jumbotron>

  <div class="mt-3">
    <b-button-group>
        <b-button 
        variant="success" 
        @click="prev"
        :disabled="isFirstQuestion">Previous
        </b-button>

        <b-button 
        variant="primary" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered || attempted.get(questionIndex)">
        {{attempted.get(questionIndex)?'Submitted':'Submit'}}
        </b-button>

        <b-button 
        variant="success" 
        @click="next"
        :disabled="isLastQuestion">Next
        </b-button>
    </b-button-group>
  </div> 
    
</div>

</template>



<script>
import _ from 'lodash'

export default {
   props: {
    currentQuestion: Object,
    questionIndex: Number,
    next: Function,
    prev: Function,
    isLastQuestion: Boolean,
    isFirstQuestion: Boolean,
    increment: Function
  },

  data(){
    return{
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
      attempted: new Map()
      
    }
  },

  mounted(){
    this.shuffleAnswers();
  },

  computed: {
  answers(){
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push[this.currentQuestion.correct_answer];
      console.log("answers: "+answers);
      //console.log(this.questionIndex());
      return answers;
    }
  },

  watch: {
    currentQuestion: {
      immediate: true,
      handler(){
      this.selectedIndex = null;
      this.answered = false;
      this.shuffleAnswers();
      console.log(this.currentQuestion);
      console.log("questionIndex: "+this.questionIndex);
      }
    }
    
  },

  methods: {
     
    selectAnswer(index){
      this.selectedIndex = index;
      console.log(index);
    },

    submitAnswer(){
      let isCorrect = false;
      if(this.selectedIndex === this.correctIndex){
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
      this.attempted.set(this.questionIndex, true);
      console.log("Attempted Questions: "+ this.attempted.entries());
      
    },

    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
      console.log("shuffleAnswers: "+answers);
     
    },

    answerClass(index){
      let answerClass = '';
      
      console.log("Attempted Questions: "+ this.attempted.size);
      if(!this.answered && this.selectedIndex === index){
        answerClass = 'selected';
      }else if(this.answered && this.correctIndex === index ){
        answerClass = 'correct';
      }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index ){
        answerClass = 'incorrect';
      }
    
      return answerClass;     
    }
  }

}
</script>



<style>
.list-group{
  margin-bottom:15px;
}

.list-group-item:hover{
  background:linen;
  cursor: pointer;
  color:black;
}

.btn{
  margin: 0 5px;
}

.selected{
  background-color:lightskyblue;
  color:ivory;
}

.correct{
  background-color:limegreen;
  color:ivory;
}

.incorrect{
  background-color: crimson;
  color:ivory;
}

</style>

