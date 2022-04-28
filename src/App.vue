<template>
  <MyElevator
    v-for="(elevator, elevatorIndex) in elevators"
    :key="elevator.elevatorId"
    :elevator="elevator"
    :nrOfFloors="nrOfFloors"
    @buttonPressed="calledFromElevator($event, elevatorIndex)"
  />
  <div class="up-down-btns">
    <div
      v-for="(upDwnBtn, index) in upDwnBtns"
      :key="upDwnBtn.id"
      class="up-down-btn"
    >
      <div>
        <div>{{ upDwnBtns.length - 1 - index }}</div>
        <button @click="btnPress('up', index)">↑</button>
        <button @click="btnPress('down', index)">↓</button>
      </div>
    </div>
  </div>
</template>

<script>
import MyElevator from "./components/MyElevator.vue";
import { defineComponent, ref } from "vue";

export default defineComponent({
  components: {
    MyElevator,
  },
  setup() {
    const nrOfFloors = ref(9);
    const upDwnBtns = ref([]);
    for (let i = 0; i < nrOfFloors.value; i++) {
      upDwnBtns.value.push({ isPressed: false, id: i });
    }
    const elevators = ref([
      {
        currentFloor: 0,
        isMoving: false,
        callQueue: [],
        elevatorId: 0,
      },
      {
        currentFloor: nrOfFloors.value - 1,
        isMoving: false,
        callQueue: [],
        elevatorId: 1,
      },
      /*{
        currentFloor: nrOfFloors.value - 2,
        isMoving: false,
        callQueue: [],
        elevatorId: 2,
      }*/
    ]);
    function btnPress(direction, index, elevatorIndex) {
      console.log(direction, index);
      const indexOfElevator = elevatorIndex ?? getClosestElevator(index);
      elevators.value[indexOfElevator].callQueue.push(index);
      console.log(indexOfElevator);
      elevators.value[indexOfElevator].currentFloor = index;
    }

    function getClosestElevator(callFloor) {
      const elevatorsDistance = elevators.value.map((elevator) => {
        return Math.abs(elevator.currentFloor - callFloor);
      });
      const minDistance = Math.min(...elevatorsDistance);
      const indexOfElevator = elevatorsDistance.indexOf(minDistance);
      return indexOfElevator;
    }

    function calledFromElevator({ index }, elevatorIndex) {
      //console.log(index, elevatorIndex);
      btnPress("up", index, elevatorIndex);
    }
    
    return {
      calledFromElevator,
      btnPress,
      elevators: elevators.value.reverse(),
      nrOfFloors,
      upDwnBtns: upDwnBtns.value.reverse(),
    };
  },
});
</script>

<style>
#app {
  display: flex;
}
button {
  cursor: pointer;
}
button:disabled {
  cursor: auto;
}
pre {
  position: absolute;
  top: 6rem;
  left: 34px;
}
.pre-f {
  left: 200px;
}
.floor {
  border: 1px solid #b2b2b2;
  display: grid;
  grid-template-columns: 3fr 1fr;
  place-content: center;
  max-width: 24rem;
  margin: 0 auto;
  height: 15rem;
  position: relative;
}

.passing.floor::after {
  content: "";
  position: absolute;
  top: 20px;
  left: 50px;
  width: 20px;
  height: 20px;
}

.doors {
  height: 11rem;
  border: 1px solid;
  width: 20rem;
  margin: 0 auto;
  position: inherit;
  top: 5%;
  left: 0%;
  display: flex;
  justify-content: space-between;

  transition: all 1s ease-in-out;
}

.door {
  background-color: rgb(255, 199, 199);
  z-index: 2;
  transition: width 1s ease-in-out;
}
.door:nth-child(1),
.door:nth-child(2) {
  height: 10.9rem;
  border: 1px solid;
  width: 10rem;
}

.open .door:nth-child(1) {
  width: 1rem;
}
.open .door:nth-child(2) {
  width: 1rem;
}

.btns {
  justify-content: flex-start;
  display: flex;
  align-items: center;
}

.numered-btns {
  opacity: 1;
  margin: 4.2rem 0.2rem;
  display: flex;
  flex-direction: column;
}

.numered-btns.appear {
  opacity: 1;
}

.elevator-buttons {
  vertical-align: 0%;
  z-index: 1;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: center;
}

.number {
  border-radius: 50%;
  border: 1px solid;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  width: 1.2rem;
  height: 1.2rem;
}

.up-down-btns {
    display: inline-block;
    position: absolute;
    left: 800px;
}

.up-down-btn {
  position: relative;
  height: 240px;
  align-items: stretch;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.elevatorsPosition {
  text-align: center;
  width: 15px;
  height: 20px;
  align-self: top;
  border: 1px solid black;
  padding: 1px;
  margin: 1px;
  font-weight: bold;
  background-color: rgb(240, 30, 30);
}
.active.elevatorsPosition{
  background-color: rgb(107, 223, 54);
}
</style>