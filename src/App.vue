<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    question: 'Что такое Vue?',
    answer: 0,
    options: ['Фронтенд фреймворк', 'Библиотека', 'Язык программирования'],
    selected: null,
  },
  {
    question: 'Что такое Vuex?',
    answer: 1,
    options: ['Npm пакет', 'Библиотека управления состоянием', 'Javascript фреймворк'],
    selected: null,
  },
  {
    question: 'Что такое Vue SFC?',
    answer: 2,
    options: ['Виртуальный DOM', 'Экземпляр приложения', 'Формат файла для конкретной платформы'],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const SetAnswer = (event) => {
  questions.value[currentQuestion.value].selected = event.target.value;
  event.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  }
  quizCompleted.value = true;
};
</script>

<template>
  <main class="app">
    <h1>Vue quiz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">Счет {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected === index
              ? index === getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected !== null && index !== getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index === questions.length - 1
            ? 'Завершить'
            : getCurrentQuestion.selected === null
            ? 'Выберите ответ'
            : 'Следующий вопрос'
        }}
      </button>
    </section>
    <section v-else>
      <h2>Вы закончили викторину</h2>
      <p>Ваш счет {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}
body {
  background-color: #271c36;
  color: #fff;
}

</style>
