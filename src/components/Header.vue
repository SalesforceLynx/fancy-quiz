<template>
<div>
  <b-nav tabs>
    <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
    <b-nav-item disabled> Score: {{numCorrect}}/{{totalQuestions}} </b-nav-item>
    <b-nav-item disabled> Attempted: {{numTotal}} </b-nav-item>
    <b-nav-item disabled> Correct: <b>{{numCorrect}}</b> </b-nav-item>
    <b-nav-item disabled> Incorrect: <b>{{numTotal - numCorrect}}</b> </b-nav-item>
    <b-nav-item disabled> Total Questions: <b>{{totalQuestions}}</b> </b-nav-item>
  </b-nav>
</div>
</template>

<script>
export default {
  props:[
    'numCorrect',
    'numTotal',
    'totalQuestions'
  ]
  
  
}
</script>