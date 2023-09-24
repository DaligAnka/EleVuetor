<style  scoped src="../components/styles/styles-cabins.css"></style>

<script setup>
import CabinLevelPanel from './CabinLevelPanel.vue';


const props = defineProps({
  currentFloor: Number,
  numfloors:Number,
});

</script>


<template>
    <div class="cab"  :style="cabStyle" > 
<CabinLevelPanel :currentFloor="props.currentFloor"> </CabinLevelPanel></div>
</template>




