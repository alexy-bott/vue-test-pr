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

</script>

<template>
  <Header :score="score"/>
  <Card v-for="(card, index) in cards"
        :key="index"
        :card-word="card.word"
        :card-translation="card.translation"
        :card-num="index + 1"
        />
  <Button>Начать игру</Button>
</template>
<style scoped>
</style>
