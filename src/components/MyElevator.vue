<template>
  <div class="container">
    <div class="floor" v-for="floor in floors" :key="floor.id">
      <div class="doors">
        <div class="door"></div>
        <div class="door"></div>
        <div class="elevator-buttons">
          <div class="numered-btns">
            <button class="number">123</button>
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
    const floors = ref([]);
    for (let i = 0; i < props.nrOfFloors; i++) {
      floors.value.push({ isHere: i === props.elevator.currentFloor, isPassing: false });
    }
    console.log(props.elevator);
    console.log(floors.value)
    return {
      floors,
    };
  },
};
</script>

<style>
</style>