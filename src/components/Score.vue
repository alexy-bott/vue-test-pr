<script setup>
import HeartIcon from "./icons/HeartIcon.vue";

const {stat} = defineProps({
  stat: {
    type: Number,
    default: 0
  }
})
</script>

<template>
  <div class="header-stat">
    {{ stat }}
    <HeartIcon/>
  </div>
</template>

<style scoped>
.header-stat {
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-sizing: border-box;
  border-radius: 36px;
  padding: 12px 24px;
  min-width: 110px;
  height: 48px;
  box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.15);
  font-weight: 700;
  font-size: 16px;
  line-height: 1;
  background: var(--blue-100);
  color: var(--blue-600);
}
</style>