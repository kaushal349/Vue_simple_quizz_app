<template>
  <div id="app">
    <Header :correctans = "correctans" 
      :totalans="totalans"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <Question 
          v-if="questions.length"
          :current_question="questions[index]"
          :next="next"
          :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Question from './components/QuestionBox.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Header,
    Question,
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctans: 0,
      totalans: 0,
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(iscorrect){
      if(iscorrect){
        this.correctans++
      }
      this.totalans++
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple',{
      method: 'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData=>{
      this.questions = jsonData.results
    }))
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
