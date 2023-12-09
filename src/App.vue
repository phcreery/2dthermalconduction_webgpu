<template>
  <canvas id="canvas" width="400" height="400"></canvas>
  {{ fps }}
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { Simulator } from './ThermalConduction/Simulator'

let simulation = null
let fps = ref(0)
async function startProgram() {
  // Create program
  let canvas = document.getElementById('canvas')
  console.log(canvas)
  simulation = new Simulator(canvas)

  // Start
  await simulation.initialize()
  // for (let i = 0; i < 20; i++) {
  // await simulation.tick(0.1);
  loop()
  // }
}

let lastT = Date.now() / 1000
async function loop() {
  let newT = Date.now() / 1000
  let dt = newT - lastT
  lastT = newT

  // Tick for simulation
  await simulation.tick(dt)

  // calculate fps
  // let fps = 1 / dt;
  // console.log(fps);
  fps.value = 1 / dt

  // Request next frame
  requestAnimationFrame(loop)
}

// window.addEventListener("load", startProgram);

onMounted(() => {
  startProgram()
})
</script>

<style scoped></style>
