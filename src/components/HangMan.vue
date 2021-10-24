<template>
    <h1>Find the word</h1>
    <div>
        <p id="to-find">
            <span :class="{ letter: letter !== '_' }" v-for="letter in hiddenWord">{{ letter }}</span>
        </p>
        <div v-if="!hasWon">
            <p>Failed letters</p>
            <p id="failed-letters">
                <span class="failed-letter new-letter" v-for="letter in failedLetter">{{ letter }}</span>
            </p>
        </div>
        <p v-if="hasWon">You won !</p>
        <div v-if="hasLost">
        <p>You lost :(</p>
        <p>The word was {{toFind}}</p>
            </div>
        <button @click="reset">Reset game</button>
    </div>
</template>

<script setup>
import { computed, reactive, ref, onMounted } from "vue";

const toFind = ref("baguette");
const currentLetter = ref("");
const submittedLetters = reactive([]);

const hiddenWord = computed(() => toFind.value.split("").map(letter => submittedLetters.includes(letter) ? letter : "_"))
const failedLetter = computed(() => submittedLetters.filter(letter => !toFind.value.includes(letter)))
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


<style>
body {
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    background-color: antiquewhite;
    text-align: center;
}
#to-find {
    font-size: 5em;
    letter-spacing: 0.1em;
}

#failed-letters {
    font-size: 3em;
    letter-spacing: 0.2em;
    color: red;
}

.letter {
    animation-name: new-letter;
    animation-duration: 0.25s;
}

.failed-letter {
    animation-name: failed-letter;
    animation-duration: 0.8s;
    display: inline-block;
}

@keyframes new-letter {
    from {
        font-size: 1.5em;
    }
}

@keyframes failed-letter {
    0% {
        transform: skewX(-15deg);
    }
    5% {
        transform: skewX(15deg);
    }
    10% {
        transform: skewX(-15deg);
    }
    15% {
        transform: skewX(15deg);
    }
    20% {
        transform: skewX(0deg);
    }
    100% {
        transform: skewX(0deg);
    }
}
</style>