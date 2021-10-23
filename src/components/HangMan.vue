<template>
    <h1>Hangman</h1>
    <div>
        <p>Find the word</p>
        <p>{{ hiddenWord.join(" ") }}</p>
        <p v-if="!hasWon">Failed letters: {{ failedLetter.join(" ") }}</p>
        <p v-if="hasWon">You won !</p>
        <p v-if="hasLost">You lost :(</p>
        <button @click="reset">Reset game</button>
    </div>
</template>

<script setup>
import { computed, reactive, ref, onMounted } from "vue";

const toFind = ref("baguette");
const currentLetter = ref("");
const submittedLetters = reactive([]);

const hiddenWord = computed(() => toFind.value.split("").map(letter => submittedLetters.includes(letter) ? letter : "_"))
const failedLetter = computed(() => submittedLetters.filter(letter => !toFind.value.includes(letter)).sort())
const hasWon = computed(() => toFind.value.split("").filter(letter => !submittedLetters.includes(letter)).length === 0)
const hasLost = computed(() => failedLetter.value.length >= 8)

const isLetter = (c) => c.toLowerCase() != c.toUpperCase()

const handleKeyPress = event => {
    if (hasLost.value || hasWon.value) return;
    const pressed = event.key.toLowerCase();
    if (isLetter(pressed) && !submittedLetters.includes(pressed)) {
        submittedLetters.push(pressed)
    }
}
onMounted(() => window.addEventListener("keypress", handleKeyPress))


const reset = () => {
    currentLetter.value = ""
    submittedLetters.splice(0);
}

</script>
