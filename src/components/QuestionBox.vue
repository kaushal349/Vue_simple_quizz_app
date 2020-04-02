<template>
    <div class="question-box-container">
        <b-jumbotron >
            <template v-slot:lead>
            {{current_question.question}}
            </template>

            <hr class="my-4">

            <!-- <p v-for="(answer, index) in answers" :key="index">
                {{answer}}
            </p> -->

            <div class="list-group">
                <div class="list-group-item" v-for="(answer,idx) in shuffled_answers" 
                :key="idx" 
                @click.prevent="select_answer(idx)" 
                :class="answerClass(idx)"
                >
                    {{answer}}
                </div>
            </div>

            <b-button variant="primary" class='btn mr-3' 
            @click="submit_answer" 
            :disabled="selected_index === null || answered"
            >
                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props :{
        current_question : Object,
        next : Function,
        increment : Function
    },
    data(){
        return {
            selected_index : null,
            shuffled_answers : [],
            answered: false,
            correct_index: null,
        }
    },
    methods :{
        select_answer(index){
            this.selected_index = index
        },
        shuffle_answers(){
         let ans = [...this.current_question.incorrect_answers,this.current_question.correct_answer]
         this.shuffled_answers = _.shuffle(ans)
         this.correct_index = this.shuffled_answers.indexOf(this.current_question.correct_answer)
        },
        submit_answer(){
            let iscorrect = false
            if(this.shuffled_answers[this.selected_index] === this.current_question.correct_answer)
            {
                iscorrect = true
            }
            this.answered = true

            this.increment(iscorrect)
        },
        answerClass(index){
            let ans_class = ''
            if(!this.answered && this.selected_index === index){
                ans_class =  'selected'
            }
            else if(this.answered  && this.correct_index === index){
                ans_class = 'correct'
            }
            else if(this.answered && this.selected_index === index && this.shuffled_answers[this.selected_index]!== this.current_question.correct_answer){
                ans_class = 'incorrect'
            }
            return ans_class
        }
    },
    // watch can be used to get to know whenever anything under props changes
    watch : {  
        current_question : {
            immediate : true,
            handler(){
                this.selected_index = null
                this.shuffle_answers()
                this.answered = false
            }
        }
    },
    computed:{
        // answers() {
        //     let ans = [...this.current_question.incorrect_answers]
        //     ans.push(this.current_question.correct_answer)
        //     return ans
        //     // return this.shuffled_answers
        // }
    },
}
</script>

// complete classic bootstrap will work! scoped means we want to apply these styles to this template only!
<style scoped> 
.list-group{
    margin-bottom : 20px;
}
.list-group-item:hover{
    background: #EEE;
    cursor: pointer;
}

.selected{
    background: lightblue;
}

.correct{
    background: lightgreen;
}

.incorrect{
    background: red;
}

</style>