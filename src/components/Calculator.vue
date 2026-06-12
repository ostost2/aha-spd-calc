<script setup lang="ts">
import { computed, reactive, ref } from 'vue'

const randomHsl = () => `hsla(${Math.random() * 360}, 80%, 50%, 1)`

const list = reactive([{spd: 100, color: randomHsl()}]);

const sorted = computed(() => [...list].sort((a, b) => b.spd - a.spd))

const speed = computed(() => {
  let spd = 80;
  let scale = 5;
  for (let i of sorted.value) {
    spd += i.spd / scale;
    scale *= 2;
  }
  return isNaN(spd) ? "?" : spd.toFixed(1);
})
</script>

<template>
  <section class="center">
    <h1 class="title">Aha's speed</h1>
    <div id="display">{{ speed }}</div>
    <div id="inputs">
      <p id="label">Elation teammate's<br>speed values:</p>
      <div>
        <div v-for="(entry, index) in list">
          <input class="field" type="number" :value="entry.spd" :style="{color: entry.color}"
            @input="e => list[index].spd = Number.parseFloat((e.target as HTMLInputElement)?.value)">
          <input class="delete" v-if="index > 0" type="button" value="×" @click="list.splice(index, 1)">
        </div>
        <div v-if="list.length < 4"><input class="field" type="button" value="+" @click="_ => list.push({spd: 100, color: randomHsl()})"></div>
      </div>
    </div>

    <p>Add your roster's speed values in any order</p>
    <p>80<template v-for="(entry, index) in sorted"> + <span :style="{color: entry.color}">{{ entry.spd }}</span> / {{ 5 * 2 ** index }}</template> = <b>{{ speed }}</b></p>
  </section>
</template>
