<template>
    <div>
        <div v-html="currentQuestion.question" class="question">
        </div>
        <div class="container">
            <div v-for="(column, index) in displayAnswers" 
                :key="index" 
                :class="index ? 'column-2' : 'column-1'" 
                class="column-padding"
            >
                <div v-for="(answer, index) in column" 
                    :key="index" 
                    v-html="answer.value" 
                    @click="selectAnswer(answer)" 
                    :class="['answer', { correct: correctStyling(answer), incorrect: incorrectStyling(answer)}]" 
                />
            </div>
        </div>
    </div>
</template>

<script>
import _ from "lodash"

export default {
    props: {
        questions: {
            type: Array,
            required: true,
            default: () => []
        }
    },
    data() {
        return {
            currentQuestion: {},
            currentQuestionNumber: 1,
            currentAnswer: {},
            score: 0,
            disable: false
            
        };
    },
    computed: {
        randomAnswers() {
            let mappedIncorrect = _.map(this.currentQuestion.incorrect_answers, (value, index) => {
                return { id: index,  value: value, correct: false }
            })
            let answers = [...mappedIncorrect, { id: 4, value: this.currentQuestion.correct_answer, correct: true }]
            return _.shuffle(answers)
        },
        displayAnswers() {
            return _.chunk(this.randomAnswers, 2)
        },
        answerSelected() {
            return !_.isEmpty(this.currentAnswer)
        }
    },
    created() {
        this.currentQuestion = this.questions[0]
    },
    methods: {
        selectAnswer(answer) {
            if(!this.disable)
                this.recordAnswer(answer)
        },
        recordAnswer(answer) {
            this.disable = true
            this.currentAnswer = answer
            if(this.isCorrect(answer))
                this.score++
            if(this.currentQuestionNumber === 10) {
                this.$emit('score', this.score)
            } else {
                setTimeout(() => {
                this.currentQuestion = this.questions[this.currentQuestionNumber]
                this.currentQuestionNumber++
                this.currentAnswer = {}
                this.disable = false;
                }, 1300)
            }
        },
        isSelected(answer) {
            return answer.id === this.currentAnswer.id
        },
        isCorrect(answer) {
            return answer.id === 4
        },
        correctStyling(answer) {
            return this.answerSelected && this.isCorrect(answer)
        },
        incorrectStyling(answer) {
            return this.isSelected(answer) && !this.isCorrect(answer)
        }
    }
}
</script>

<style scoped>
.question {
    background-color: var(--main-primary-light);
    width: 80%;
    margin: auto;
    text-align: center;
    padding: 3em 0px;
    border-radius: var(--border-radius);
    margin-top: 50px;
}
.container {
    width: 80%;
    display: grid;
    grid-template-columns: 50% 50%;
    margin: auto;
}
.column-padding {
    padding: 0px 20px;
}
.column-1 {
    grid-column: 1;
}
.column-2 {
    grid-column: 2;
}
.answer {
    width: 100%;
    height: 9em;
    background-color: var(--main-secondary-light);
    margin: auto;
    margin-top: 40px;
    overflow: scroll;
    line-height: 9em;
    border-radius: var(--border-radius);
    transition: 0.3s;
    text-align: center;
}
.answer:hover {
    background-color: var(--main-white);
    color: var(--main-secondary-light);
    cursor: pointer;
}
.correct {
    background-color: var(--main-accent-success) !important;
    color: var(--main-white) !important;
}
.incorrect {
    background-color: var(--main-accent-failure) !important;
    color: var(--main-white) !important;
}
.hidden {
    display: none;
}

</style>