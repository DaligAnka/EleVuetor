<script setup>


import Shafts from './components/Shafts.vue'
import Cabins from './components/Cabins.vue';
import Floors from './components/Floors.vue';

import { ref} from 'vue';
import { Num_Floor} from './config';


const ElevuetorState =(index) => {
    const storedState = JSON.parse(
        localStorage.getItem(`ElevuetorState-${index}`) || '{}'
    );

return {
    currentFloor: ref(storedState.currentFloor || 1)
};
};

const createNewCab = (index) => {
const state = ElevuetorState(index);

const newCab ={
        ...state
    };

return newCab;
};

const saveState = (newCab,index) => {

    const state = {
        currentFloor: newCab.currentFloor.value
    };
    localStorage.setItem(`ElevuetorState-${index}`, JSON.stringify(state));

};


</script>

<template>



<div class="Shaft-Container"> 
<Shafts> </Shafts>
</div>

<Floors :numfloors="Num_Floor"></Floors>

<div class="Cabins-Container">
<Cabins  ></Cabins>
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
