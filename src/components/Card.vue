<script setup lang="ts">
import { ref, defineProps, defineEmits } from 'vue';
const props = defineProps({
    imgBackFaceUrl: {
        type: String,
        required: true,
    },
    card: {
        type: [String, Number, Array, Object]
    },
    cardsContext: {
        type: Array<number>,
        default: () => [],
    },
})
const isFlipped = ref(false)
const isDisabled = ref(false)
const emit = defineEmits(['onFlip']);
const onToggleFlipCard = () => {
    if (isDisabled.value) {
        return false;
    }
    isFlipped.value = !isFlipped.value;
    if (isFlipped.value) {
        emit('onFlip', props.card)
    }
}
const onFlipBackCard = () => {
    isFlipped.value = false;
}
const onEnabledDisabledMode = () => {
    isDisabled.value = true;
}

defineExpose({ onFlipBackCard, onEnabledDisabledMode });

</script>

<template>
    <div class="card" :class="{ disabled: isDisabled }" :style="{
        height: `${(920 - 16 * 4) / Math.sqrt(props.cardsContext.length) - 16}px`,
        width: `${((920 - 16 * 4) / Math.sqrt(props.cardsContext.length) - 16) * 0.75}px`,
        perspective: `${((920 - 16 * 4) / Math.sqrt(props.cardsContext.length) - 16) * 1.5}px`,
    }">
        <div class="card__inner" :class="{ 'is-flipped': isFlipped }" @click="onToggleFlipCard">
            <div class="card__face card_face--front">
                <div class="card__content" :style="{
                    'background-size': `${((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 0.25}px 
                                            ${((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 0.25}px`,
                }"></div>
            </div>
            <div class="card__face card_face--back">
                <div class="card__content" :style="{ backgroundImage: `url('${props.imgBackFaceUrl}')` }"></div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
}

.card__inner {
    widows: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card__inner.is-flipped {
    transform: rotateY(-180deg);
}

.card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card_face--front .card__content {
    background: url("/images/icon_back.png") no-repeat center center;
    width: 100%;
    height: 100%;
}

.card_face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}

.card_face--back .card__content {
    width: 100%;
    height: 100%;
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
}

.card.disabled .card__inner {
    cursor: default;
}
</style>