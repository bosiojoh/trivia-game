<template>
    <div class="container">
        <h1>Game Over</h1>
        <p class="score">
            {{ scoreMessage }}
        </p>
        <button @click="playAgain" class="button">Play Again!</button>
    </div>
</template>

<script>
export default {
    props: {
        correctAnswers: {
            type: Number,
            required: true,
            default: 0
        }
    },
    computed: {
        scoreMessage() {
            return 'You answered ' + this.correctAnswers + ' out of 10 questions correctly'
        }
    },
    methods: {
        playAgain() {
            this.$emit('playAgain')
        }
    }
}
</script>

<style scoped>
.container {
    text-align: center;
}
.body {
    width: 50%;
    background-color: var(--main-secondary-light);
    margin: auto;
    border-radius: var(--border-radius);
    height: 500px;
}
.score {
    font-size: 2em;
}
.button {
    background-color: var(--main-secondary-light);
    display: block;
    margin: auto;
    border: none;
    padding: 1.5em 9em;
    margin-top: 50px;
    border-radius: var(--border-radius);
    transition: 0.3s;
    color: var(--main-white);
    cursor: pointer;
    font-size: 1em;
}
</style>