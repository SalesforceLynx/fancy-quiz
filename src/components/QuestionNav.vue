<template>
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
        :disabled="selectedIndex === null || answered">Submit
        </b-button>

        <b-button 
        variant="success" 
        @click="next"
        :disabled="isLastQuestion">Next
        </b-button>
    </b-button-group>
  </div>
</template>


<script>
export default {
    props: {
         next: Function,
         prev: Function,
         isLastQuestion: Boolean,
         isFirstQuestion: Boolean
         //increment: Function
    },

    data(){
      return{
        selectedIndex: null,
        correctIndex: null,
        answered: false
      }
    },

    methods: {
        submitAnswer(){
        let isCorrect = false;
        if(this.selectedIndex === this.correctIndex){
            isCorrect = true;
        }
        this.answered = true;
        this.increment(isCorrect);

        }
    }
    
  }

</script>



<style>
.list-group{
  margin-bottom:15px;
}

.list-group-item:hover{
  background: #eee;
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

