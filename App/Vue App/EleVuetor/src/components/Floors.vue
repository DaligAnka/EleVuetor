<style scoped  src="../components/styles/styles-floors.css" ></style>

<script setup>
const props = defineProps({
numfloors: Number,
callNewCab: Function,
});
</script>


<template>
  <div class="floors">
    <div v-for="floor in props.numfloors" :key="floor" class="floor" >
      <div class="floorLevel">{{ floor }}</div>
        <button class="callBtn" v-on:click="() => callNewCab (floor)"> 
          <div class="callBtn1" ></div>
           </button>
      </div>
  </div>
</template>
