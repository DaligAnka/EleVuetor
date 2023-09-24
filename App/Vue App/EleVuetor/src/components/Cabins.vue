<style  scoped src="../components/styles/styles-cabins.css"></style>

<script setup>
import { computed } from 'vue';
import CabinLevelPanel from './CabinLevelPanel.vue';


const props = defineProps({
  currentFloor: Number,
  direction: String,
   targetfloor: Number,
  activeCalls: Array,
  numfloors:Number,
});

const isResting = computed(()=> props.activeCalls[props.currentFloor - 1]);

const cabStyle = computed(()=> {
  const floorHeigt = 100;
  const translateY = (props.currentFloor - 1) * floorHeigt;
  const cabHeight = 100 / props.numfloors;

return{
  transform: `translateY(-${translateY}%)`,
  height: `${cabHeight}%`,
};
});

</script>


<template>
    <div class="cab"  :style="cabStyle" :class="{resting: isResting}"  > 
<CabinLevelPanel :currentFloor="props.targetfloor" :direction="props.direction"> </CabinLevelPanel></div>
</template>




