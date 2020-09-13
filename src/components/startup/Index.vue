<template>
    <div class="container">
        <div v-if="screen === 0">
            <h1>Trivia Game</h1>
        </div>
        <div v-else-if="screen === 1" class="category">
            <span>Category:</span>
            <Dropdown :options="categories" @dropdownSelect="updateCategory" key="category" class="dropdown" />
        </div>
        <div v-else-if="screen === 2" class="difficulty">
            <span>Difficulty:</span>
            <Dropdown :options="difficulties" @dropdownSelect="updateDifficulty" key="difficulty" class="dropdown" />
        </div>
        <div v-else>
            <p class="settings">{{"Category- " + category.name}}</p>
            <p class="settings">{{"Difficulty- " + difficulty.name}}</p>
            <button v-if="canBegin" @click="beginTrivia" class="button">Begin</button>
            <button @click="reselect" class="button">Reselect <i class="fas fa-undo-alt"></i></button>
        </div>
        <button @click="forward" v-if="showContinue" class="button">Continue <i class="fas fa-arrow-right"></i></button>
    </div>
</template>

<script>
import Dropdown from "./Dropdown.vue"
import _ from "lodash"

export default {
    components: {
        Dropdown
    },
    props: {
        categories: {
            type: Array,
            required: true,
            default: () => []
        }
    },
    data() {
        return {
            difficulties: [{id: 1, name: 'Easy'}, {id: 2, name: 'Medium'}, {id: 3, name: 'Hard'}],
            category: {},
            difficulty: {},
            screen: 0
        };
    },
    computed: {
        canBegin() {
            return !_.isEmpty(this.category) && !_.isEmpty(this.difficulty)
        },
        showContinue() {
            switch(this.screen) {
                case 0: return true;
                case 1: return !_.isEmpty(this.category);
                case 2: return !_.isEmpty(this.difficulty);
                case 3: return false;
                default: return false;
            }
        }
    },
    methods: {
        updateCategory(category) {
            this.category = category
            this.$emit('updateCategory', category)
        },
        updateDifficulty(difficulty) {
            this.difficulty = difficulty
            this.$emit('updateDifficulty', difficulty)
        },
        beginTrivia() {
            this.$emit('begin')
        },
        forward() {
            this.screen++;
        },
        reselect() {
            this.difficulty = {};
            this.category = {};
            this.screen = 1
        }
    }
}
</script>

<style scoped>
.dropdown {
    display: inline-block;
    margin: 0px 20px;
}
.container {
    text-align: center;
    margin-top: 50px;
}
.button {
    background-color: var(--main-secondary-light);
    display: block;
    border: none;
    /* padding: 1.5em 9em; */
    width: 400px;
    height: 4em;
    margin: 25px auto 0px auto;
    border-radius: var(--border-radius);
    transition: 0.3s;
    color: var(--main-white);
    cursor: pointer;
    font-size: 1em;
    font-weight: bolder;
}
.begin:hover {
    background-color: var(--main-white);
    color: var(--main-secondary-light);
}
.category {
    display: inline-block;
    z-index: 1;
}
.difficulty {
    display: inline-block;
    z-index: 2;
}
.dropdowns {
    margin: auto;
    display: inline-block;
}
.settings {
    font-size: 2em;
    margin: 5px 0px;
}
</style>