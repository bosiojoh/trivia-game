<template>
    <div>
        <div v-if="!loading">
            <transition name="fade" mode="out-in">
                <StartIndex v-if="screen === 0" 
                        @updateCategory="setCategory" 
                        @updateDifficulty="setDifficulty" 
                        @begin="beginTrivia"
                        :categories="allCategories"
                        />
                <GameIndex v-else-if="screen === 1"  :questions="questions" @score="saveScore"/>
                <EndIndex v-else :correctAnswers="score" @playAgain="playAgain"/>
            </transition>
        </div>
        <VueElementLoading :active="loading" :is-full-screen="true" color="#5D737E" background-color="#3F4045"/>
    </div>
</template>

<script>
import StartIndex from './startup/Index.vue';
import GameIndex from './game/Index.vue';
import EndIndex from './end/Index.vue';
import VueElementLoading from 'vue-element-loading';
import _ from 'lodash';

export default {
    components: {
        StartIndex,
        GameIndex,
        EndIndex,
        VueElementLoading
    },
    data() {
        return {
            loading: true,
            questions: [],
            screen: 0,
            categories: [],
            selectedCategory: {},
            selectedDifficulty: {},
            score: 0
        };
    },
    computed: {
        allCategories() {
            return [{id: 99, name: "Any"}, ...this.categories]
        },
        query() {
            const params = new URLSearchParams();
            if(this.selectedCategory.id !== 99)
                params.append('category', this.selectedCategory.id);
            params.append('difficulty', _.lowerCase(this.selectedDifficulty.name));
            return params;
        }
    },
    created() {
        this.getCategories()
    },
    methods: {
        getQuestions() {
            this.$http
                .get('https://opentdb.com/api.php?amount=10&type=multiple', { 
                    params: this.query
                })
                .then(response => {
                    this.questions = response.data.results
                    this.screen = 1;
                    this.loading = false
                })
                .catch(err => {
                    console.log(err)
                })
        },
        getCategories() {
            this.$http
                .get('https://opentdb.com/api_category.php')
                .then(response => {
                    this.categories = response.data.trivia_categories
                    this.loading = false
                })
        },
        setCategory(category) {
            this.selectedCategory = category;
        },
        setDifficulty(difficulty) {
            this.selectedDifficulty = difficulty;
        },
        beginTrivia() {
            this.loading = true
            this.getQuestions();
        },
        saveScore(score) {
            this.score = score
            this.screen = 2;
        },
        playAgain() {
            this.questions = [],
            this.screen = 0,
            this.selectedCategory = {},
            this.selectedDifficulty = {},
            this.score = 0
        }
    }
}
</script>

<style scoped>
.header {
    color: var(--main-white);
}
</style>