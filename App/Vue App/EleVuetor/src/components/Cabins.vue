<style  scoped src="../components/styles/styles-cabins.css"></style>

<script setup>
import { computed } from 'vue';
import CabinLevelPanel from './CabinLevelPanel.vue';


const props = defineProps({
  currentFloor: Number,
  direction: String,
});



const cabStyle = computed(()=> {
  const floorHeigt = 100;
  const translateY = (props.currentFloor - 1) * floorHeigt;

return{
  transform: `translateY(-${translateY}%)`,
};
});

</script>


<template>
    <div class="cab"  :style="cabStyle"  > 
<CabinLevelPanel :currentFloor="props.targetfloor" :direction="props.direction"> </CabinLevelPanel></div>
</template>




