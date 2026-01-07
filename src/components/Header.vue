<script setup>
import Score from "./Score.vue";

defineProps({
  score: {
    type: Number,
    default: 0
  }
});
</script>

<template>
  <header class="header" aria-label="Количество очков">
    <h1 class="header-title">запомни слово</h1>
    <Score :stat="score"/>
  </header>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 40px 65px;
  max-width: 1440px;
  box-sizing: border-box;
  margin: 0 auto;
}

.header-title {
  margin: 0;
  font-family: var(--font);
  font-weight: 700;
  font-size: 16px;
  letter-spacing: 0.12em;
  color: var(--black);
  text-transform: uppercase;
}
</style>