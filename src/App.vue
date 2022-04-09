<template>
  <MyElevator
    v-for="elevator in elevators"
    :key="elevator.id"
    :elevator="elevator"
    :nrOfFloors="nrOfFloors"
  />
  <div class="btns">
    <div v-for="upDwnBtn in upDwnBtns" :key="upDwnBtn.id" class="up-down-btns">
      <div>{{upDwnBtn.id}}</div>
      <button @click="btnPress">↑</button>
      <div>{{upDwnBtn.isPressed}}</div>
      <button @click="btnPress">↓</button>
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
    const nrOfFloors = ref(7);
    const upDwnBtns = ref([]);
    for ( let i = 0; i < nrOfFloors.value; i++ ) {
      upDwnBtns.value.push({isPressed:false,id:i})
    }
    console.log(upDwnBtns.value);
    const elevators = ref([
      {
        currentFloor: 0,
        isMoving: false,
      },
      {
        currentFloor: nrOfFloors.value - 1,
        isMoving: false,
      },
    ]);
    function btnPress (){
      console.log(upDwnBtns.value)
    }
    return {
      btnPress,
      elevators,
      nrOfFloors,
      upDwnBtns,
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
  max-width: 34rem;
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
  top: 10%;
  left: 15%;
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
  margin: 1rem 1rem;
  display: flex;
  flex-direction: column;
}

.numered-btns.appear {
  opacity: 1;
}

.elevator-buttons {
  z-index: 1;
  position: absolute;
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
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>