<script setup>
import Button from "./components/Button.vue";
import Header from "./components/Header.vue";
import Card from "./components/Card.vue";
import {onMounted, ref} from "vue";

const API_WORDS = 'http://localhost:8080/api/random-words'

// ref для хранения очков
const score = ref(0);

// ref для хранения массива карт
const cards = ref([])

const fetchCards = async () => {
  const response = await fetch(API_WORDS)
  const dataCards = await response.json()

cards.value = dataCards.map((card) => ({
  word: card.word,
  translation: card.translation,
  state: "closed",
  status: "pending",
}))
}

onMounted(()=> {
  fetchCards()
})

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
