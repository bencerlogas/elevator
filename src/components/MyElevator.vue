<template>
  <div class="container">
    <div class="floor" v-for="(floor, index) in floors" :key="floor.id">
      <div class="doors" :class="elevator.currentFloor === index && !elevator.isMoving ? 'open' : ''">
        <div class="door"></div>
        <div class="door"></div>
        <div class="elevator-buttons">
          <div class="numered-btns" v-for="(numeredBtn, index) in numeredBtns.slice()" :key="numeredBtn.id">
            <button @click="numBtnPress(index)" class="number">{{numeredBtn.id}}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  props: {
    elevator: {
      type: Object,
      required: true,
    },
    nrOfFloors: {
      type: Number,
      required: true,
    },
  },
  setup(props) {
    let elev = ref(props.elevator);
    console.log(elev);
    const numeredBtns = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      numeredBtns.value.push({ id: i });
    }
    console.log(numeredBtns.value);
    const floors = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      floors.value.push({ isHere: i === props.elevator.currentFloor, isPassing: false });
    }
    // nmBtnPress calls elevator to the floor
    function numBtnPress(index) {
      elev.value.callQueue.reverse();
      elev.value.callQueue.push(index);
      elev.value.currentFloor = index;
    }
    console.log(props.elevator);
    console.log(floors.value)
    return {
      numBtnPress,
      numeredBtns,
      floors,
    };
  },
};
</script>

<style>
</style>