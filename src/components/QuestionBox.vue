<template>
    <div class="question-box-container">
        <b-jumbotron >
            <template #lead>
            {{currentQuestion.question}}
            </template>

            <hr class="my-4">
            <b-list-group>
            <b-list-group-item 
            v-for="(answers,index) in shuffledAnswers" 
            :key="index"
            @click.prevent="selectedAnswer(index)"
            :class="answerClass(index)"
            >
                {{answers}}
            </b-list-group-item>
            
            </b-list-group>
            

            <b-button variant="primary" href="#"
            @click="submitAnswer"
            :disabled="selectedIndex===null || answered">
            Submit
            </b-button>

            <b-button variant="success" href="#" 
            @click='next'
            :disabled="!answered"
            >
                Next
            </b-button>
        </b-jumbotron>
    </div>
    
</template>

<script>
import _ from 'lodash'
export default{
    props:{
        currentQuestion:Object,
        next:Function,
        increment:Function,
    },
    data()
    {
        return{
            shuffledAnswers:[],
            selectedIndex:null,
            correctIndex:null,
            answered:false
        }
    },
    methods:{
        shuffledAnswer()
        {
            let answer=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
            this.shuffledAnswers= _.shuffle(answer);
            this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        selectedAnswer(index)
        {
            this.selectedIndex=index;
        },
        submitAnswer()
        {
            let isCorrect=false;
            if(this.selectedIndex === this.correctIndex)
            {
                isCorrect=true;
            }
            this.answered=true;
            this.increment(isCorrect);
        },
        answerClass(index)
        {
            let answerClass=''
            if(!this.answered && this.selectedIndex === index)
            {
                answerClass="selected"
            }
            else if(this.answered && this.correctIndex === index)
            {
                answerClass="correct"
            }
            else if(this.answered && this.selectedIndex === index && this.correctIndex !== index)
            {
                answerClass="wrong"
            }
            return answerClass;
        }
    },
    watch:{
        currentQuestion:{
            immediate:true,
            handler()
            {
                this.selectedIndex=null;
                this.answered=false;
                this.shuffledAnswer();
            }

        }
    }
}
</script>
<style scoped>
.list-group{
    margin-bottom:15px;
}
.list-group-item:hover{
    background:#eee;
    cursor:pointer;
}
.btn
{
    margin:0px 10px;
}
.selected
{
    background-color:lightblue;
}
.correct
{
    background-color: lightgreen;
}
.wrong
{
    background-color: red;
}
</style>
