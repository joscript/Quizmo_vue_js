<template>
    <div class=" mt-2 bg-white">
            <div v-if="numTotal === 10">
                <div class="card p-3">
                    <h3 class="text-center text-secondary"> You got {{numCorrect}} / {{numTotal}} </h3>
                    <button class="btn btn-info" @click="reloadPage">
                        Try again
                    </button>
                </div>
            </div>
        <b-jumbotron v-else class="bg-white shadow">


            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                v-for="(answer, index) in answers" 
                v-bind:key="index"
                @click="selectAnswer(index)"
                v-bind:class="answerClass(index)"
                >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button class="m-2" 
            variant="primary" 
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered" >
                Submit
            </b-button>
            <b-button class="m-2" variant="success" @click="next" v-bind:disabled="answered === false">
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from "lodash";
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        numCorrect : Number,
        numTotal : Number
    },
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    watch: {
        currentQuestion: {
            immediate: true, 
            handler(){
                this.selectedIndex = null
                this.shuffleAnswers()
                this.answered = false
            }
        }
    },
    computed: {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
            // console.log(index)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer] 
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.increment(isCorrect)
            this.answered = true
        },
        answerClass(index) {
            let answer_class = ''
            if(!this.answered && this.selectedIndex === index){
                answer_class = 'selected'
            }else if(this.answered && this.correctIndex === index){
                answer_class = 'correct'
            }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                answer_class = 'incorrect'
            }

            return answer_class
        },
        reloadPage(){
            location.reload();
        }
    },
    // mounted(){
    //    this.shuffleAnswers()
    // }
} 
</script>

<style scoped>
    .list-group-item:hover{
        background: #86f1ff;
        cursor: pointer;
    }

    .selected {
        background-color: #00e2ff;
    }

    .correct {
        background-color: #23ff23;
    }

    .incorrect {
        background-color: #f44336;
    }

    .bg-white{
        background-color: white;
    }
</style>