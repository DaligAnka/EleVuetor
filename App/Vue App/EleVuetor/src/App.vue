<script setup>


import Shafts from './components/Shafts.vue'
import Cabins from './components/Cabins.vue';
import Floors from './components/Floors.vue';
import { ref,reactive, watch, computed,  withModifiers} from 'vue';
import { Num_Floor, Num_Cabs} from './config';
import Interaction from './components/Interaction.vue';


const ElevuetorState =(index) => {
    const storedState = JSON.parse(
        localStorage.getItem(`ElevuetorState-${index}`) || '{}'
    );

return {
    currentFloor: ref(storedState.currentFloor || 1),
    targetFloor: ref(storedState.targetFloor || 1),
    callQueue: ref(storedState.callQueue || []),
};
};

const createNewCab = (index) => {
const state = ElevuetorState(index);

const newCab ={
      
        ...state,
      direction: ref(''),
activeCalls: reactive(new Array(Num_Floor).fill(false)),
arrivedFloors: reactive(new Array(Num_Floor).fill(false)),
movingToTarget:ref(false),
    };


return newCab;
};

const saveState = (newCab,index) => {

    const state = {
        currentFloor: newCab.currentFloor.value,
    targetFloor: newCab.targetFloor.value,
    callQueue: newCab.callQueue.value,

    };
    localStorage.setItem(`ElevuetorState-${index}`, JSON.stringify(state));

};

const delay = (ms) => new Promise((resolve) => setTimeout(resolve,ms));

 const moveNewCabToTarget = async (newCab) => {
    while (newCab.currentFloor.value !== newCab.targetFloor.value) {
        newCab.direction.value = newCab.targetFloor.value > newCab.currentFloor.value ? 'Up:': 'Down:';
        newCab.currentFloor.value += newCab.direction.value === 'Up:' ? 1 : -1;
        await delay (1000);
    }
};

const operateNewCab = (newCab, index) => {
  const callNewCab = async (floor) => {
if ((newCab.currentFloor.value === floor && !newCab.movingToTarget.value)||
(newCab.targetFloor.value === floor && newCab.movingToTarget.value) ||
newCab.callQueue.value.includes(floor)
      ) {
          return;
      }

      newCab.callQueue.value.push(floor);
    newCab.activeCalls[floor - 1] = true;
    saveState(newCab,index);

    if (newCab.movingToTarget.value) return;
while (newCab.callQueue.value.length > 0) {
    newCab.movingToTarget.value = true;
    newCab.targetFloor.value = newCab.callQueue.value[0];
    newCab.activeCalls[newCab.targetFloor.value - 1] = true;

await moveNewCabToTarget(newCab);


newCab.arrivedFloors[newCab.targetFloor.value - 1] = true;
await delay (3000);
newCab.activeCalls[newCab.targetFloor.value - 1] = false;
newCab.arrivedFloors[newCab.targetFloor.value - 1] = false;
newCab.callQueue.value.shift();
newCab.movingToTarget.value =  false;
saveState(newCab,index);

}
};
return callNewCab;

};

const newCabs = new Array (Num_Cabs).fill(null).map((_, index) => {
    const newCab = createNewCab(index);
    const callNewCab = operateNewCab(newCab, index);
    return { ...newCab, callNewCab};
});

const callNewCab = async (floor) => {
    const availableCab = newCabs.filter(
        (newCab) => !newCab.movingToTarget.value
    );

const nearestCab = availableCab.reduce((nearest, newCab) => {
    if(
        !nearest || Math.abs(newCab.currentFloor.value - floor) < Math.abs(nearest.currentFloor.value - floor)
    ) {
 return newCab;
 }
      return nearest;
}, null);

    const newCabToUse = nearestCab || newCabs[0];
    await newCabToUse.callNewCab(floor);
};

const allActiveCalls = computed(() => {
    return newCabs.map((newCab) => newCab.activeCalls).reduce((totalcalls,calls) => {
        return totalcalls.map((active,index) => active || calls[index]);
    });
});

</script>

<template>



<div class="Shaft-Container"> 
<Shafts v-for="(newCab,index) in newCabs" :key="index"></Shafts>
</div>

<Floors :numfloors="Num_Floor" :callNewCab="callNewCab" :activeCalls="allActiveCalls" :arrivedFloors="newCabs[0].arrivedFloors"></Floors>

<Interaction></Interaction>

<div class="Cabins-Container">
<Cabins :numfloors="Num_Floor" v-for="(newCab,index) in newCabs"  :key="index" :currentFloor="newCab.currentFloor.value" :direction="newCab.direction.value"
 :targetfloor="newCab.targetFloor.value"   :activeCalls="newCab.activeCalls" ></Cabins>
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
