<script setup>


import Shafts from './components/Shafts.vue'
import Cabins from './components/Cabins.vue';
import Floors from './components/Floors.vue';
import { ref,reactive, watch} from 'vue';
import { Num_Floor} from './config';


const ElevuetorState =(index) => {
    const storedState = JSON.parse(
        localStorage.getItem(`ElevuetorState-${index}`) || '{}'
    );

return {
    currentFloor: ref(storedState.currentFloor || 1),
    targetFloor: ref(storedState.targetFloor || 1)
};
};

const createNewCab = (index) => {
const state = ElevuetorState(index);

const newCab ={
      
        ...state,
      direction: ref(''),
    };


return newCab;
};

const saveState = (newCab,index) => {

    const state = {
        currentFloor: newCab.currentFloor.value,
    targetFloor: newCab.targetFloor.value,

    };
    localStorage.setItem(`ElevuetorState-${index}`, JSON.stringify(state));

};

const currentFloor = ref(1);
const targetFloor = reactive({value:1});
const direction=ref('');
const callNewCab = (floor) => {
    targetFloor.value = floor;
    currentFloor.value = floor;

};
watch(currentFloor, (newFloor, prevFloor) => {
    direction.value = newFloor > prevFloor ? 'Up' : "Down"
});



</script>

<template>



<div class="Shaft-Container"> 
<Shafts> </Shafts>
</div>

<Floors :numfloors="Num_Floor" :callNewCab="callNewCab"></Floors>

<div class="Cabins-Container">
<Cabins :numfloors="Num_Floor" :currentFloor="currentFloor" :direction="direction" ></Cabins>
</div>

</template>



<style scoped>

.Shaft-Container{
    display: flex;
    align-items: flex-end;
}
.Cabins-Container{
position: absolute;
  bottom: 0;
  left: 0;
  display: flex;
  align-items: flex-end;
  height: 100%;
 
}


</style>
