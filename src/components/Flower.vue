<script setup>
defineProps({class: String})
</script>

<template>
    <img src="/src/assets/flower.svg" alt="Flor" class="w-full h-full" :class="class" />

</template>
