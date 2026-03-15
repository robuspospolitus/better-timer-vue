<script setup lang="ts">
import { ref, computed } from 'vue';
const props = defineProps<{
  sections: {
    name: string,
    time: number,
    additional_time: number,
    color: string
  }[]
}>()

// Variables
const numberofsections = props.sections.length;
const progress = ref(0);
const isCounting = ref(false);
let animationId:number;
let totalseconds = 0;
let startTime = 0;
props.sections.forEach(i => totalseconds += i.time);

// functions
const progressStyle = computed(() => ({ "--progress": progress.value + "%" }));

function animate(time:number) {
  if(!isCounting.value) return;
  const elapsed = (time - startTime) / 1000;
  progress.value = elapsed / totalseconds * 100;
  if(elapsed < totalseconds) animationId = requestAnimationFrame(animate);
  else {
    progress.value = 100;
    isCounting.value = false;
  }
}

function handleTimer() {
  isCounting.value = !isCounting.value;

  if(isCounting.value) {
    startTime = performance.now();
    animationId = requestAnimationFrame(animate);
  } else cancelAnimationFrame(animationId);
}

function handleView() {
  let time = 0;
  let style = "background: conic-gradient(transparent 0.5%,";
  props.sections.forEach((section, key) => {
    style += `${section.color} ${time+0.5}% ${time + section.time/totalseconds*100-0.5}%,`;
    if(key+1 < numberofsections) style += `transparent ${time + section.time/totalseconds*100-0.5}% ${time + section.time/totalseconds*100+0.5}%,`;
    time += section.time/totalseconds*100;
  })
  style += "transparent 99.5%);";
  return style;
}
</script>

<template>
    <div id="box">
        <div class="circle" :style="handleView()"/>
        <div class="circle above" :style="progressStyle"/>
        <h2>00:00:00</h2>
    </div>
    <button @click="handleTimer">{{ isCounting ? "stop":"start" }}</button>
</template>

<style>
@property --progress {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 0%;
}
#box {
    background-color: white;
    width: calc(100vw - 4rem);
    height: calc(100vw - 4rem);
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
}
.circle {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  z-index: 1;
  transition: background 1s;
}
.circle::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
.circle::after {
  content: '';
  position: absolute;
  inset: 16px;
  border-radius: 50%;
  background: white;
  box-shadow: inset 0 0 8px rgb(85, 85, 85);
}
.above {
  position: absolute;
  opacity: 0.7;
  z-index: 2;

  background: conic-gradient(transparent var(--progress), white var(--progress));
}
h2 {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 3em;
  z-index: 3;
  font-weight: 500;
}
</style>