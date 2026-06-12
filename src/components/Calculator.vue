<script setup lang="ts">
import { computed, ref } from 'vue'

const list = ref([100]);

const speed = computed(() => {
  let sorted = [...list.value].sort((a, b) => b - a);
  let spd = 80;
  let scale = 5;
  for(let i of sorted){
    spd += i / scale;
    scale *= 2;
  }
  return isNaN(spd) ? "?" : spd;
})
</script>

<template>
  <section class="center">
    <div id="display">{{ speed }} SPD</div>
    <div id="inputs">
      <div v-for="(spd, index) in list">
        <input class="field" type="number" :value="spd"
          @input="e => list[index] = Number.parseFloat((e.target as HTMLInputElement)?.value)">
        <input
          class="delete"
          v-if="index > 0"
          type="button" value="×" @click="list.splice(index, index)">
      </div>
      <div v-if="list.length < 4"><input class="field" type="button" value="+" @click="_ => list.push(100)"></div>
      
    </div>
    <footer>Type your roster's speed values in any order</footer>
  </section>
</template>
