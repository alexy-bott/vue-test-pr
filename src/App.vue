<script setup>
import Button from "./components/Button.vue";
import Header from "./components/Header.vue";
import Card from "./components/Card.vue";
import {ref} from "vue";

// ref для хранения очков
const score = ref(0);

// ref для хранения массива карт
const cards = ref([
  {
    word: "hello",
    translation: "привет",
    state: "closed", // closed | opened
    status: "pending", // success | fail | pending
  },
  {
    word: "world",
    translation: "мир",
    state: "closed",
    status: "pending",
  },
  {
    word: "computer",
    translation: "компьютер",
    state: "closed",
    status: "pending",
  },
  {
    word: "language",
    translation: "язык",
    state: "closed",
    status: "pending",
  },
]);

const handleOpen = (cardProps) => {
  // Найти карту по номеру и открыть
  const cardIndex = cardProps.cardNum - 1;
  if (cards.value[cardIndex]) {
    cards.value[cardIndex].state = 'opened';
  }
};

const handleAction = (actionData) => {
  // Найти карту и установить результат
  const cardIndex = actionData.cardNum - 1;
  if (cards.value[cardIndex]) {
    cards.value[cardIndex].status = actionData.remembered ? 'success' : 'fail';
    // Карта остается в состоянии 'opened' - НЕ переворачиваем обратно
    if (actionData.remembered) {
      score.value += 1;
    }
  }
};
</script>

<template>
  <Header :score="score"/>
  <div class="cards-grid">
  <Card v-for="(card, index) in cards"
        :key="index"
        :card-word="card.word"
        :card-translation="card.translation"
        :card-num="index + 1"
        :state="card.state"
        :status="card.status"
        @open="handleOpen"
        @action="handleAction"
        />
  </div>
  <Button>Начать игру</Button>
</template>
<style scoped>
.cards-grid {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  padding: 20px;
  justify-content: center;
}
</style>
