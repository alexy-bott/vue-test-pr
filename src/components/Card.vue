<script setup>
import {reactive} from 'vue';
import CheckIcon from "./icons/CheckIcon.vue";
import CrossIcon from "./icons/CrossIcon.vue";

const props = defineProps({
  cardNum: {
    type: Number,
    default: 0,
  },
  cardWord: {
    type: String,
    default: "",
  }
});

const emit = defineEmits(['flip', 'action']);

const card = reactive({
  state: 'initial',  // 'initial' | 'flipped' | 'completed'
  remembered: null   // null | true | false
});

const flipCard = () => {
  card.state = 'flipped';
  emit('flip', props);
};

const handleYes = () => {
  emit('action', {...props, remembered: true});
  card.state = 'completed';
  card.remembered = true;
};

const handleNo = () => {
  emit('action', {...props, remembered: false});
  card.state = 'completed';
  card.remembered = false;
};
</script>

<template>
  <div class="card-container">
    <!-- Внутренний контейнер с 3D трансформацией -->
    <div
        class="card"
        :class="{
        'card--flipped': card.state  === 'flipped' || card.state  === 'completed',
        'card--clickable': card.state  === 'initial'
      }"
        @click="card.state === 'initial' && flipCard()"
    >
      <!-- Передняя сторона -->
      <div class="card-face card-face--front">
        <span class="card-number">{{ props.cardNum }}</span>
        <div class="card-border">
          <div class="card-content">
            {{ props.cardWord }}
          </div>
        </div>

        <div class="card-actions">
          <span class="card-actions-label">перевернуть</span>
        </div>
      </div>

      <!-- Задняя сторона -->
      <div class="card-face card-face--back">
        <span class="card-number">{{ props.cardNum }}</span>
        <div class="card-border">
          <div class="card-content">
            {{ props.cardWord }}
          </div>
        </div>

        <div class="card-actions">
          <!-- Кнопки "ДА" и "НЕТ" -->
          <template v-if="card.state === 'flipped'">
            <button
                class="card-action-button"
                @click.stop="handleNo"
                aria-label="Нет, не запомнил"
            >
              <CrossIcon/>
            </button>
            <button
                class="card-action-button"
                @click.stop="handleYes"
                aria-label="Да, запомнил"
            >
              <CheckIcon/>
            </button>
          </template>

          <!-- Надпись "ЗАВЕРШЕНО" -->
          <span
              v-else-if="card.state === 'completed'"
              class="card-actions-label"
          >
            завершено
          </span>
        </div>

        <div v-if="card.state === 'completed'" class="check-status-icon">
          <CheckIcon v-if="card.remembered===true"/>
          <CrossIcon v-else-if="card.remembered===false"/>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
/* Контейнер для perspective */
.card-container {
  perspective: 1000px;
  width: 250px;
  height: 376px;
}

/* Основная карточка с 3D трансформацией */
.card {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.6s cubic-bezier(0.4, 0.0, 0.2, 1);
}

/* Состояние переворота */
.card--flipped {
  transform: rotateY(180deg);
}

/* Курсор для кликабельной карточки */
.card--clickable {
  cursor: pointer;
}

/* Общие стили для обеих сторон */
.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 16px;
  box-shadow: 0 0 16px 0 rgba(0, 0, 0, 0.1);
  background: var(--white);
  transition: box-shadow 0.3s ease;
}

.card-face--front:hover,
.card--flipped .card-face--back:hover {
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.15);
}

/* Передняя сторона */
.card-face--front {
  transform: rotateY(0deg);
}

/* Задняя сторона */
.card-face--back {
  transform: rotateY(180deg);
}

/* Hover эффект для передней стороны */
.card--clickable .card-face--front .card-border {
  transition: background 0.2s ease;
}

.card--clickable .card-face--front:hover .card-border {
  background: rgba(0, 139, 254, 0.02);
}

.card--clickable .card-face--front:active .card-border {
  background: rgba(0, 139, 254, 0.05);
}

/* Номер карточки */
.card-number {
  position: absolute;
  top: 18px;
  left: 35px;
  background: var(--white);
  color: var(--gray-900);
  z-index: 1;
  padding: 0 12px;
  font-family: var(--font);
  font-weight: 600;
  font-size: 16px;
}

/* Рамка карточки */
.card-border {
  position: relative;
  width: 212px;
  height: 320px;
  border: 1px solid var(--blue-100);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Контент */
.card-content {
  font-family: var(--font);
  font-weight: 400;
  font-size: 18px;
  text-align: center;
  color: var(--black);
  padding: 20px;
}

/* Блок действий */
.card-actions {
  position: absolute;
  bottom: 19px;
  left: 50%;
  transform: translateX(-50%);
  padding: 0 12px;
  background: var(--white);
  z-index: 1;
  display: flex;
  gap: 32px;
  align-items: center;
}

/* Надписи "ПЕРЕВЕРНУТЬ" и "ЗАВЕРШЕНО" */
.card-actions-label {
  font-family: var(--font);
  font-weight: 600;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--black);
  white-space: nowrap;
}

/* Кнопки */
.card-action-button {
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.card-action-button:hover {
  transform: scale(1.1);
  background: rgba(9, 187, 0, 0.1);
}

.card-action-button:active {
  transform: scale(0.95);
}

.check-status-icon {
  position: absolute;
  top: 9px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--white);
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 39px;
  height: 39px;
}
</style>