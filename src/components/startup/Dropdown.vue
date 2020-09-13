<template>
  <div @mouseover="showOptions = true" @mouseleave="showOptions = false" class="container">
    <div class="selected-box" :class="{'selected-box-open': showOptions}" >
        <span class="selected">
            {{ this.displaySelected }}
        </span>
        <i class="fas fa-chevron-down down-arrow"></i>
    </div>
    <div v-if="showOptions" class="all-options" @mouseover="showOptions = true" @mouseleave="showOptions = false">
        <div v-for="option in options" 
            :key="option.id" 
            @click="onSelect(option)" 
            class="option"
            :class="{'option-selected': isSelected(option)}"
        >
            {{ option.name }}
        </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash"

export default {
    props: {
        options: {
            type: Array,
            required: true,
            default: () => []
        }
    },
    data() {
        return {
            selected: {},
            showOptions: false
        };
    },
    computed: {
        displaySelected() {
            return _.isEmpty(this.selected) ? "Select an option..." : this.selected.name
        }
    },
    methods: {
        onSelect(option) {
            this.selected = option
            this.$emit('dropdownSelect', option )
        },
        isSelected(option) {
            return option.id === this.selected.id
        }
    }

}
</script>

<style scoped>
.selected-box {
    background-color: var(--main-white);
    padding: 1em 0px;
    display: grid;
    grid-template-columns: 90% 10%;
    border-radius: var(--border-radius);
    border-bottom: 1px solid var(--main-secondary-dark);
    width: 100%;
}
.selected-box-open {
    border-radius: var(--border-radius) var(--border-radius) 0px 0px;
}
.down-arrow {
    grid-column: 2;
    margin-top: 5px;
}
.container {
    width: 250px;
    color: var(--main-secondary-light);
    position: relative;
}
.option {
    padding: 1em;
    transition: 0.3s;
    cursor: pointer;
}
.option-selected {
    background-color: var(--main-primary-light) !important;
    color: var(--main-white) !important;
}
.option:hover {
    background-color: lightgray;
}
.all-options {
    background-color: var(--main-white);
    border-radius: 0px 0px var(--border-radius) var(--border-radius);
    max-height: 400px;
    overflow: scroll;
    position: absolute;
    width: 100%;
}
.selected {
    color: var(--main-secondary-light);
    grid-column: 1;
    margin-left: 10px;
}
</style>