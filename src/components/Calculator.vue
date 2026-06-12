<script setup lang="ts">
import { computed, ref } from 'vue'

const list = ref([100]);

const sorted = computed(() => [...list.value].sort((a, b) => b - a))

const speed = computed(() => {
  let spd = 80;
  let scale = 5;
  for (let i of sorted.value) {
    spd += i / scale;
    scale *= 2;
  }
  return isNaN(spd) ? "?" : spd;
})
</script>

<template>
  <section class="center">
    <h1 class="title">Aha's speed</h1>
    <div id="display">{{ speed }}</div>
    <div id="inputs">
      <p id="label">Elation teammate's<br>speed values:</p>
      <div>
        <div v-for="(spd, index) in list">
          <input class="field" type="number" :value="spd"
            @input="e => list[index] = Number.parseFloat((e.target as HTMLInputElement)?.value)">
          <input class="delete" v-if="index > 0" type="button" value="×" @click="list.splice(index, index)">
        </div>
        <div v-if="list.length < 4"><input class="field" type="button" value="+" @click="_ => list.push(100)"></div>
      </div>
    </div>

    <p>Add your roster's speed values in any order</p>
    <p>80<template v-for="(spd, index) in sorted"> + {{ spd }} / {{ 5 * 2 ** index }}</template> = <b>{{ speed }}</b></p>
  </section>
</template>
