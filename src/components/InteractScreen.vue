<script setup lang="ts">
import { ref, getCurrentInstance  } from 'vue';
import CardFlip from './Card.vue';

const props = defineProps({
    cardsContext: {
        type: Array<number>,
        default: () => [],
    },
})

interface rulesType {
    index: number
    value: number,
}

const rules = ref<rulesType[]>([]);
const { refs } = getCurrentInstance();
const checkRule = (card: rulesType) => {
    console.log(card);
    
    if (rules.value.length === 2) {
        return false
    }
    rules.value.push(card)
    
    if(rules.value.length === 2 && rules.value[0].value === rules.value[1].value) {
        console.log("right...");
    } else if(rules.value.length === 2 && rules.value[0].value !== rules.value[1].value) {
        console.log("wrong...");
        const firstCardRef = `card-${rules.value[0].index}`;
        console.log(refs);
        
        const firstCardComponent = refs[firstCardRef] as { onFlipBackCard: () => void } | undefined;
        if (firstCardComponent) {
            firstCardComponent.onFlipBackCard();
        }

        const secondCardRef = `card-${rules.value[1].index}`; 
        const secondCardComponent = refs[secondCardRef] as { onFlipBackCard: () => void } | undefined;
        if (secondCardComponent) {
            secondCardComponent.onFlipBackCard();
        }
        
        //close card

    } else {
        return false;
    }
}
</script>

<template>
    <div class="scre">
        <h1>Bắt đầu chơi</h1>
        <CardFlip v-for="(card, index) in props.cardsContext" :key="index" :imgBackFaceUrl="`/images/${card}.png`"
            :card="{index, value: card}" @onFlip="checkRule({index, value: card})"
            :ref="`card-${index}`" />
    </div>
</template>

