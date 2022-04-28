<template>
  <div class="container">
    <div class="floor" v-for="(floor, index) in floors" :key="floor.id">
      <div class="elevatorsPosition"
           :class="elevator.currentFloor === index ? 'active' : ''"
      >
        {{ nrOfFloors - elevator.currentFloor - 1 }}
        </div>
      <div
        class="doors"
        :class="
          elevator.currentFloor === index && !elevator.isMoving ? 'open' : ''
        "
      >
        <div class="door"></div>
        <div class="door"></div>
        <div class="elevator-buttons">
          <div
            class="numered-btns"
            v-for="(numeredBtn, index) in numeredBtns"
            :key="numeredBtn.id"
          >
            <button
              @click="
                numBtnPress(numeredBtns.length - 1 - index, numeredBtn);
              "
              class="number"
            >
              {{ numeredBtn.id }}
            </button>
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
  emits: ["buttonPressed"],
  setup(props, { emit }) {
    const numeredBtns = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      numeredBtns.value.push({
        id: i,
        numeredBtnPressed: false,
        thisElevator: props.elevator.elevatorId,
      });
    }
    const floors = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      floors.value.push({
        isHere: i === props.elevator.currentFloor,
        isPassing: false,
      });
    }
    function switchValue(index) {
      numeredBtns.value[index].numeredBtnPressed =
        !numeredBtns.value[index].numeredBtnPressed;
    }
    function numBtnPress(index, btnNumber) {
      emit("buttonPressed", { index, btnNumber });
    }
    return {
      switchValue,
      numBtnPress,
      numeredBtns,
      floors,
    };
  },
};
</script>

<style>
</style>