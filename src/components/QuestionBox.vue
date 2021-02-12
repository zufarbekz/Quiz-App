<template>
   <div>
  <b-jumbotron>
    <template #lead>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">
    <div> 
    <b-list-group class="list-group">
        <b-list-group-item class="list-item"
            v-for="(answer, index) in answers"
            v-bind:key = "index"
            v-on:click="selectAnswer(index)"
            :class="answerClass(index)">
                {{answer}}
        </b-list-group-item>
    </b-list-group>

    </div>

    <div class="btn">
        <b-button variant="primary"  
        :disabled="selectedIndex === null || answered"
         v-on:click="submitAnswer">
         Submit
         </b-button>

        <b-button variant="success"  
        @click="next">
        Next
        </b-button>
    </div>

  </b-jumbotron>
</div>
</template>

<!--
We need this in order to say "Hey i am receving variable from the parent"
props - is needed when we pass variable from parent(App) to its child(QuestionBox)
-->
<script>
import _ from 'lodash';

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctIndex: 0 
        } 
    },
    computed:{
        answers(){
            let answers = [... this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    watch: {
        currentQuestion(){
            this.selectedIndex = null;
            this.answered = false;
            this.shuffleAnswers();
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index;
        },
        submitAnswer(){
            let isCorrect = false;
            if(this.currentQuestion.correct_answer === this.shuffledAnswers[this.selectedIndex]){
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        shuffleAnswers(){
             let answers = [... this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
             this.shuffledAnswers = _.shuffle(answers);
             this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        answerClass(index){
            let answerClass = 
            [
                !this.answered && this.selectedIndex === index ? 'selected' : 
                this.answered && this.correctIndex === index ? 'correct' :
                this.answered && this.selectedIndex === index && this.correctIndex !== index ? 'wrong' : ''
            ]
            return answerClass;
        }
    },
    mounted(){
        this.answered = false;
        this.shuffleAnswers(this.answers);
    }
}
</script>

<style scoped>
.list-group{
    cursor: pointer;
}
.list-item:hover{
   background-color: #ddd;
}
.selected{
    background-color: lightblue;
}
.correct{
    background-color: lightgreen;
}
.correct:hover{
    background-color: lightgreen;
}
.wrong{
    background-color: rgb(212, 34, 34);
}
.wrong:hover{
    background-color: rgb(185, 55, 55);
}
.btn{
    margin: 15px 5px;
}
</style>