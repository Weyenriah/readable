<script setup lang="ts">
import { ref, watch } from 'vue'

const props = defineProps<{
    LIX: number
}>()

const difficulty = ref<string>('easy')

const round = (num: number): number => {
    return Math.round((num + Number.EPSILON) * 10) / 10
}

/**
 * Check difficulty.
 * <30 = easy
 * 30-50 = medium
 * >50 = hard
 *
 * @param LIX LIX score
 */
const setDifficulty = (LIX: number): void => {
    if(LIX < 30) {
        difficulty.value = 'easy'
    } else if(LIX >= 30 && LIX <= 50) {
        difficulty.value = 'normal'
    } else {
        difficulty.value = 'hard'
    }
}

/**
 * Watch LIX score to change difficulty.
 */
watch(() => props.LIX, (LIX: number): void => {
    setDifficulty(LIX)
})
</script>

<template>
    <div class="LIXScore">
        <p class="LIXScore__heading">
            LIX:
        </p>
        <p
            class="LIXScore__score"
            :class="'LIXScore__score--' + difficulty"
        >
            {{ round(LIX) }}
        </p>
    </div>
</template>

<style lang="scss" scoped>
.LIXScore {
    display: flex;
    align-items: center;

    &__heading {
        margin-right: .2rem;
        font-size: 1.2rem;
    }

    &__score {
        border-radius: 30px;
        width: fit-content;
        padding: 0.5rem 1rem;
        margin: 0;
        font-size: 1.2rem;
        color: white;

        &--easy {
            background-color: #009b29;
        }

        &--normal {
            background-color: #ff9500;
        }

        &--hard {
            background-color: #e20000;
        }
    }
}
</style>