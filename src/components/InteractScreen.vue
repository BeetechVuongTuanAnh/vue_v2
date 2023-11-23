<script setup lang="ts">
import { ref } from 'vue';
import CardFlip from './Card.vue';

interface rulesType {
    index: number
    value: number,
}

const props = defineProps({
    cardsContext: {
        type: Array<number>,
        default: () => [],
    },
})

const emit = defineEmits(['onFinish']);
const cardItem = ref<any>([]);
const rules = ref<rulesType[]>([]);
const checkRule = (card: rulesType) => {
    if (rules.value.length === 2) {
        return false
    }
    rules.value.push(card)
    if (rules.value.length === 2 && rules.value[0].value === rules.value[1].value) {
        cardItem.value[rules.value[0].index].onEnabledDisabledMode();
        cardItem.value[rules.value[1].index].onEnabledDisabledMode();
        rules.value = [];
        const disabledElements = document.querySelectorAll(".screen .card.disabled");
        if (disabledElements && disabledElements.length === props.cardsContext.length - 2) {
            setTimeout(() => {
                emit('onFinish')
            }, 920)
        }
    } else if (rules.value.length === 2 && rules.value[0].value !== rules.value[1].value) {
        setTimeout(() => {
            cardItem.value[rules.value[0].index].onFlipBackCard();
            cardItem.value[rules.value[1].index].onFlipBackCard();
            rules.value = []
        }, 800)
    } else {
        return false;
    }
}
</script>

<template>
    <div class="screen">
        <div class="screen__inner" :style="{
            width: `${(((920 - 16 * 4) / Math.sqrt(props.cardsContext.length) - 16) * 0.75 + 16) * Math.sqrt(props.cardsContext.length)}px`
        }">
            <CardFlip v-for="(card, index) in props.cardsContext" :key="index" :imgBackFaceUrl="`/images/${card}.png`"
                :card="{ index, value: card }" @onFlip="checkRule({ index, value: card })" ref="cardItem"
                :cardsContext="cardsContext" />
        </div>
    </div>
</template>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
}

.screen__inner {
    width: calc(424px);
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>

