<script setup>
import {onMounted, ref} from 'vue'

const currentDate = ref('')

onMounted(() => {
  currentDate.value = new Date().toLocaleDateString('ru-RU', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
})
</script>

<template>
  <div>
    <p> Сегодня: {{ currentDate }}</p>
  </div>
</template>

<style scoped>
</style>
