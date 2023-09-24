<style scoped src="../components/styles/styles-levelPanel.css"></style>

<script setup>
const props = defineProps({
    currentFloor:Number,
    direction: String
});
</script>


<template>
    <div class="levelPanel">
        <div class="direction">{{ direction }}</div>
        <div class="target">{{ currentFloor }} </div>
    </div>
</template>