<template>
    <h1>Hangman</h1>
    <div>
        <p>Find the word</p>
        <p>{{ hiddenWord }}</p>
        <p>Failed letters: {{ failedLetter }}</p>
        <form @submit.prevent="handleForm">
            <input type="text" v-model="currentLetter" maxlength="1" />
            <button>Send letter</button>
        </form>
        <p v-if="hasWon"> You won !</p>
        <button @click="reset">Reset game</button>
    </div>
</template>

<script setup>
import { computed, reactive, ref } from "vue";

const toFind = ref("baguette");
const currentLetter = ref("");
const submittedLetters = reactive([]);

const hiddenWord = computed(() => toFind.value.split("").map(letter => submittedLetters.includes(letter) ? letter : "_").join(" "))
const failedLetter = computed(() => submittedLetters.filter(letter => !toFind.value.includes(letter)).sort().join(" "))
const hasWon = computed(() => toFind.value.split("").filter(letter => !submittedLetters.includes(letter)).length === 0)

const handleForm = () => {
    if (!submittedLetters.includes(currentLetter.value)) submittedLetters.push(currentLetter.value);
    currentLetter.value = "";
}

const reset = () => {
    currentLetter.value = ""
    submittedLetters.splice(0);
}

</script>
