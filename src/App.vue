<script lang="ts" setup>
import { computed, ref } from 'vue';

const RESET = {
 quantity: 2,
 cols: 2,
 level: 1,
 finish: false,
};

const startParams = ref({
 ...RESET,
 heightField: 60,
});

const randomField = computed(() => {
 return Math.floor(Math.random() * startParams.value.quantity + 1);
});

function checkChoice(field: number) {
 if (field === randomField.value) {
  switch (true) {
   case startParams.value.quantity < 8:
    startParams.value.quantity += 2;
    break;
   case startParams.value.quantity < 40:
    startParams.value.quantity += 4;
    startParams.value.cols = 4;
    break;
   default:
    alert("You've won! Congratulations!");
    Object.assign(startParams.value, { ...RESET });
    return;
  }
  startParams.value.level += 1;
 } else {
  startParams.value.finish = true;
  setTimeout(() => {
   Object.assign(startParams.value, { ...RESET });
  }, 1000);
 }
}
</script>

<template>
 <div class="pt-[20px]">
  <h1 class="text-[30px] text-center font-700 mb-[30px]">
   Find Field — Level {{ startParams.level }}
  </h1>
  <div
   class="w-full max-w-[800px] grid border mx-auto"
   :class="startParams.finish ? 'pointer-events-none' : ''"
   :style="`grid-template-columns: repeat(${startParams.cols}, 1fr)`"
  >
   <div
    @click="checkChoice(field)"
    v-for="field in startParams.quantity"
    :key="field"
    class="border cursor-pointer"
    :class="startParams.finish ? 'bg-[red]' : ''"
    :style="`height: ${startParams.heightField}px;`"
   ></div>
  </div>
 </div>
</template>

<style scoped></style>
