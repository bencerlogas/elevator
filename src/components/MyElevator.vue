<template>
  <div class="container">
    <div class="floor" v-for="(floor, index) in floors" :key="floor.id">
      <div class="currentFloor">{{ nrOfFloors - elevator.currentFloor - 1 }}</div>
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
    //const elev = ref(props.elevator);
    const numeredBtns = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      numeredBtns.value.push({
        id: i,
        numeredBtnPressed: false,
        thisElevator: props.elevator.elevatorId,
      });
    }
    //console.log(numeredBtns.value);
    const floors = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      floors.value.push({
        isHere: i === props.elevator.currentFloor,
        isPassing: false,
      });
    }
    // emit numBtnPress(index) to parent
    function switchValue(index) {
      numeredBtns.value[index].numeredBtnPressed =
        !numeredBtns.value[index].numeredBtnPressed;
      //console.log(numeredBtns.value);
      //return numeredBtns.value[index].numeredBtnPressed;
      //numeredBtnPressed.value = true;
      //emit("numButton", index,numeredBtns);
    }
    //console.log(numeredBtns.value);
    // cycle through numeredBtns and check if any of them is pressed
    // if so, emit to parent
    function numBtnPress(index, btnNumber) {
      emit("buttonPressed", { index, btnNumber });
      // for (let i = 0; i < numeredBtns.value.length; i++) {
      //   if (numeredBtns.value[i].numeredBtnPressed) {
      //     emit("numButton", numeredBtns.value[i].thisElevator, numeredBtns.value[i].id );
      //     console.log(numeredBtns.value[i].id);
      //     numeredBtns.value[i].numeredBtnPressed = false;
      //   }
      // }
    }
    /*function numBtnPress(index) {
      emit(numBtnPress(index));
    }*/
    /*function numBtnPress(index) {
      elev.value.callQueue.push(index);
      elev.value.currentFloor = index.reverse();
    }*/
    // for cycle through numeredBtns and check if the button is pressed, if so, emit to the parent the index of button and thisElevator value
    /*for(let i = 0; i < numeredBtns.value.length; i++) {
      if(numeredBtns.value[i].numeredBtnPressed) {
        emit("numButton", i,numeredBtns.value[i].numeredBtnPressed,numeredBtns.value);
      }
    }*/
    //console.log(numeredBtns.value);
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