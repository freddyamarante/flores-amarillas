<script setup>
import { ref, nextTick } from 'vue'
import Flower from './components/Flower.vue'
import { useElementSize } from '@vueuse/core'
import { gsap } from 'gsap'

const flowers = ref([])
const flowerCount = ref(0)

const container = ref(null)
const { width, height } = useElementSize(container)

function addFlower() {
  flowerCount.value++
  const newFlower = {
    id: flowers.value.length,
    top: Math.random() * height.value,
    left: Math.random() * width.value,
    size: Math.random() * (300 - 50) + 50
  }
  flowers.value.push(newFlower)


  nextTick(() => {
    const flowerElement = document.querySelectorAll('.box')[flowers.value.length - 1]
    gsap.fromTo(flowerElement, 
      { scale: 0, opacity: 0 }, 
      { scale: 1, opacity: 1, duration: 0.8, ease: 'bounce.out' } // Pop effect with a bounce
    )
  })
}
</script>


<template>
  <div ref="container" class="flex justify-center items-center w-screen h-screen overflow-hidden">
    <button 
      v-if="flowerCount < 200"
      class="relative z-40 px-4 py-2 rounded-md text-center bg-orange-300 hover:bg-neutral-600 text-neutral-900 hover:text-neutral-50 font-semibold text-2xl" 
      @click="addFlower"

    >
      Añade una flor
    </button>


    <Flower 
      v-for="flower in flowers" 
      :key="flower.id" 
      class="box"
      :style="{ 
        position: 'absolute', 
        top: `${flower.top}px`, 
        left: `${flower.left}px`,
        width: `${flower.size}px`,
        height: `${flower.size}px`
      }" 
    />

    <span class="absolute z-40 bottom-12 right-12 rounded-full bg-slate-50 text-slate-900 py-2 px-4 text-2xl">
      {{ flowerCount }} flores
    </span>
    
    <div v-if="flowerCount >= 200" class="absolute z-50 w-screen h-screen flex justify-center items-center bg-transparent backdrop-brightness-50">
      <div class="bg-slate-50 text-slate-900 p-4 lg:p-24 max-w-sm lg:max-w-full rounded-3xl">
        <h2 class="text-2xl font-semibold text-center">¡Ya no puedes añadir más flores!</h2>
        <p class="text-lg">Has llegado al límite de 200 flores. No sé para qué quieres tantas flores pinche hoe.</p>
        <h3 class="mt-8 text-right text-sm">Te amo muchisimo, atentamente: Ate</h3>
      </div>
    </div>
  </div>
</template>

