<template>
  <button class="button">
    <slot/>
  </button>
</template>

<script>

</script>

<style scoped>
.button {
  border: none;
  color: var(--white);
  background: var(--blue-600);
  border-radius: 100px;
  font-family: var(--font);
  font-weight: 400;
  font-size: 24px;
  line-height: 36px;
  letter-spacing: normal;
  padding: 16px 10px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 335px;
  min-height: 68px;
}

.button:hover {
  background: var(--blue-700);
}
</style>