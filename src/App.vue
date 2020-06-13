<template>
  <div id="app">
    <Header
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    :totalQuestions="totalQuestions"/>
    
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">

          <QuestionBox 
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :questionIndex="index"
          :increment="increment"
          :next="next"
          :prev="prev"
          :totalQuestions="totalQuestions"
          :isLastQuestion="isLastQuestion"
          :isFirstQuestion="isFirstQuestion"/>
          
        </b-col>
      </b-row>
    </b-container>  

    <!-- <QuestionNav
    v-if="questions.length" 
    :next="next"
    :prev="prev"
    :totalQuestions="totalQuestions"
    :isLastQuestion="isLastQuestion"
    :isFirstQuestion="isFirstQuestion"/>  -->
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
//import QuestionNav from './components/QuestionNav.vue'


export default {
 
  name: 'App',
  components: {
    Header,
    QuestionBox
    //QuestionNav
  },

  data(){
    return{
      questions: [],
      //indexedQuestions: {id: Number, question: String},
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      questionCounter: [],
      totalQuestions: null,
      isLastQuestion: false,
      isFirstQuestion: true
    }

  },

  methods:{

    // allQuestions(){
    //    let thisQuestion = {id: Number, question: String};
    //    thisQuestion.question = JSON.parse(JSON.stringify(this.currentQuestion.question));
      
    //   console.log("thisQuestion: "+JSON.stringify(thisQuestion));
    //   return JSON.stringify(thisQuestion);
    // },

    next(){
      this.index++;
      this.questionCounter.push(this.index);
      this.isLastQuestion = (this.index + 1) % this.totalQuestions ? false : true;
      this.isFirstQuestion = this.index === 0 ? true : false;
    },

    prev(){
      this.index--;
      this.isFirstQuestion = (this.index !== 0) ? false : true;
      this.isLastQuestion = (this.index + 1) % this.totalQuestions ? false : true;
     
      console.log("questionCounter length: "+ this.questionCounter.length);
      console.log("totalQuestions: "+ this.totalQuestions);
      console.log("isFirstQuestion: "+this.isFirstQuestion+" index: "+this.index);
    },

    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++;
      }
      this.numTotal++;
    }

  },

  mounted: function(){
     fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
       method: 'get'
     })
     .then((response)=>{
       return response.json();
     })
     .then((jsonData)=>{
       this.questions = jsonData.results; 
       this.totalQuestions = this.questions.length;
       console.log("Total Questions: "+this.totalQuestions);
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
  color: #2c3e50;
  margin-top: 60px;
}
</style>
