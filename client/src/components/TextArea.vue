<script setup lang="ts">
    import { ref } from 'vue'
    import LIXScore from '@/components/LIXScore.vue'

    const text = ref<string>('')
    const LIX = ref<number>(0)
    const copyConfirmed = ref<boolean>(false)

    /**
     * Calculate LIX score.
     */
    const calculateLIX = (): void => {
        // List of sentences
        const sentences: Array<string> = text.value.split(/[.!?]/)

        // Create an array of words
        const words: Array<string> = text.value.split(' ')

        // List words with more than 6 characters
        const longWords: Array<string> = words.filter(word => word.length > 6)

        // Check word difficulty
        const wordDifficulty: number = (100 * longWords.length) / words.length

        // Check sentence difficulty
        const sentenceDifficulty: number = words.length / sentences.length

        // Calculate LIX
        LIX.value = wordDifficulty + sentenceDifficulty
    }

    const copyText = (): void => {
        navigator.clipboard.writeText(text.value)
        copyConfirmed.value = true

        // Remove confirmation after 2 seconds
        setTimeout(() => {
            copyConfirmed.value = false
        }, 2000)
    }
</script>

<template>
    <div class="textarea__wrapper">
        <LIXScore :LIX="LIX" />

        <textarea
            class="textarea"
            v-model="text"
            @input="calculateLIX"
        />

        <div class="copy">
            <div
                class="copy__confirm"
                :class="{ 'copy__confirm--active': copyConfirmed }"
            >
                Yes!
            </div>

            <a class="copy__button" :class="{ 'copy__button--inactive': text === '' }" @click="copyText">
                Kopiera texten
            </a>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.textarea {
    width: 100%;
    height: 100%;
    resize: none;
    border: none;
    outline: none;
    font-size: 1.5rem;
    font-family: 'Roboto', sans-serif;
    padding: 2rem;
    background-color: transparent;
    box-shadow: 0 1px 10px 1px rgba(0, 0, 0, 0.1);
    border-radius: 30px;
}

.copy {
    position: absolute;
    bottom: 1rem;
    right: 1rem;
    display: flex;
    align-items: center;
    gap: .5rem;

    &__confirm {
        display: none;
        padding: 1rem 1.5rem;
        border-radius: 30px;
        background-color: #009b29;
        color: white;
        transition: background-color 0.2s ease-in-out;

        &--active {
            display: block;
        }
    }

    &__button {
        display: block;
        padding: 1rem 1.5rem;
        border-radius: 30px;
        background-color: #000;
        color: white;
        transition: background-color 0.2s ease-in-out;

        &:hover {
            cursor: pointer;
            background-color: #333;
        }

        &--inactive {
            pointer-events: none;
            opacity: 0.5;
        }
    }
}
</style>