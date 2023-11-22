<script setup lang="ts">
const emit = defineEmits(['onStart']);
const onStart = (totalOfBlocks:number) => {
    emit('onStart', {totalOfBlocks});
}
</script>
<template>
    <div class="screen">
        <h1>Game POKEMON</h1>
        <h3>Chọn chế độ để bắt đầu</h3>
        <div class="action">
            <button @click="onStart(16)">
                <span>4x4</span>
                <span>Dễ</span>
            </button>
            <button @click="onStart(36)">
                <span>6x6</span>
                <span>Bình thường</span>
            </button>
            <button @click="onStart(64)">
                <span>8x8</span>
                <span>Khó</span>
            </button>
            <button @click="onStart(100)">
                <span>10x10</span>
                <span>Rất khó</span>
            </button>
        </div>
    </div>
</template>