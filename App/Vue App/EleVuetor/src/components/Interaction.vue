<style scoped src="../components/styles/styles-interaction.css">
</style>


<script setup>
import { ref } from 'vue';
import { Num_Floor,Num_Cabs } from '../config';



const numFloors = ref(Num_Floor);
const numCabs = ref(Num_Cabs);

let updateFloorsPlus = () => {
    localStorage.setItem('numFloors', numFloors.value +1);
  
    location.reload();
}
let updateFloorsMinus = () => {
    localStorage.setItem('numFloors', numFloors.value -1);
  
    location.reload();
}




let updateCabsPlus = () => {
    localStorage.setItem('numCabs', numCabs.value +1);
  
    location.reload();
}
let updateCabsMinus = () => {
    localStorage.setItem('numCabs', numCabs.value -1);
  
    location.reload();
}


localStorage.setItem('numCabs', numCabs.value );

</script>


<template>
  <div class="interactions-container">
    <div class="Floors">
      <p class="changeFloors">New Floor:</p>
      <button class="btnInt" @click="updateFloorsPlus"> + </button>
        <button class="btnInt"   @click="updateFloorsMinus"> - </button>
      </div>



  
    <div class="Cabs">
      <p class="changeCabs">New Cab:</p>
      <button class="btnInt" @click="updateCabsPlus">+</button>
    <button class="btnInt" @click="updateCabsMinus">-</button>
    </div>
   
  
  </div>
</template>
