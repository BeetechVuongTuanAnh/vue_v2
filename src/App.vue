<script setup lang="ts">
import { ref } from 'vue'
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array"

const statusMatch = ref('default');

interface settingType {
    totalOfBlocks: number,
    cardsContext: Array<number>,
    startedAt: number,
}
const settings = ref<settingType>({
    totalOfBlocks: 0,
    cardsContext: [],
    startedAt: 0,
})

interface configType {
    totalOfBlocks: number
}
const onHandleBeforeStart = (config: configType) => {
    settings.value.totalOfBlocks = config.totalOfBlocks;
    const firstCards = Array.from(
        { length: settings.value.totalOfBlocks / 2 },
        (_, i) => i + 1
    );
    const secondCards = [...firstCards];
    const cards = [...firstCards, ...secondCards];
    settings.value.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
    settings.value.startedAt = new Date().getTime();
    //data ready
    statusMatch.value = 'match';
}
</script>

<template>
    <MainScreen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart" />
    <InteractScreen v-if="statusMatch === 'match'" :cardsContext = "settings.cardsContext" />
</template>
