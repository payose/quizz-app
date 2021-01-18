<template>
    <div class="question-box">
        <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">

                <template #lead>
                    {{currentQuestion.question}}
                </template>

                <hr class="my-4">

                <b-list-group>
                    <b-list-group-item 
                        v-for="(answer, index) in answers" 
                        :key="index"
                        @click="selectAnswer(index)"
                        :class="answerClass(index)"
                    >
                    {{answer}}
                    </b-list-group-item>
                
                </b-list-group>

                <b-button 
                    @click="submitAnswer"
                    :disabled="selectedIndex === null || answered"
                    variant="warning"
                >
                Submit
                </b-button>
                <b-button @click="next()" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template> 

<script>

import _ from 'lodash'

export default {

    props:{
        currentQuestion: Object,
        next: Function,
        increment:Function,
        numTotal:Object
    },

    data(){
        return{
            selectedIndex:null,
            shuffledAnswers:[],
            correctIndex:null,
            answered:false
        }
    },

    computed: {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    
    watch:{
        currentQuestion:{
            immediate:true,
            handler(){
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },

    methods:{
        selectAnswer(index){
            this.selectedIndex = index
            
        },

        submitAnswer(){
            let isCorrect = false

            if (this.selectedIndex === this.correctIndex){
                isCorrect = true
            }

            this.answered = true

            this.increment(isCorrect)

            if (this.numTotal===10){
                alert('quiz completed')
            }
        },
        
        shuffleAnswers(){
            let answers =  [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers= _.shuffle(answers)
            this.correctIndex =  this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },

        answerClass(index){
            let answerClass =''
                        
            if (!this.answered && this.selectedIndex === index){
                answerClass = 'selected'

            }

            else if (this.answered && this.correctIndex === index){
                answerClass = 'correct'
            }

            else if (this.answered && this.selectedIndex===index && this.correctIndex !== index){
                answerClass = 'incorrect'
            }

            return answerClass
        },

        
    },

    mounted(){
        this.shuffledAnswers()
    }


    
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;

}

.list-group-item:hover{
    background-color: bisque;
    cursor: pointer;
}

.btn{
    margin: 0px 5px;
}

.selected{
    background-color: cadetblue;
}

.correct{
    background-color: darkseagreen;
}

.incorrect{
    background-color: firebrick;
}
</style>