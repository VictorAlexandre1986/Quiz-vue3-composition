<template>
  <div class="quiz-container">
    <h1 class="title">Quiz App</h1>
    <div v-if="!quizStarted">
      <button class="start-button" @click="startQuiz">Start Quiz</button>
    </div>
    <div v-else>
      <h2 class="question">{{ currentQuestion.question }}</h2>
      <ul class="options">
        <li v-for="(option, index) in currentQuestion.options" :key="index">
          <button class="option-button" @click="checkAnswer(option)">{{ option }}</button>
        </li>
      </ul>
      <p class="score">Score: {{ score }}</p>
      <div v-if="situacao" class="finished"><h2>Quiz Finished!</h2><button class="option-button" @click="sair">Sair</button><button class="option-button" @click="reiniciar">Reiniciar</button> </div>
      <p v-else class="remaining">Questions remaining: {{ currentIndex }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import questions from '../questions/questions';

let currentIndex = ref(0);
let score = ref(0);
let quizStarted = ref(false);
let currentQuestion = ref(questions[currentIndex.value]);
let situacao = ref(false);


function startQuiz() {
  quizStarted.value = true;
}

function reiniciar() {
  currentIndex.value = 0;
  score.value = 0;
  quizStarted.value = false;
  currentQuestion.value = questions[0];
  situacao.value = false;
}

function checkAnswer(selectedOption) {
  if (selectedOption === currentQuestion.value.answer && currentIndex.value < (questions.length -1)) {
    score.value++;
  }
  if (currentIndex.value < (questions.length - 1)) {
    currentIndex.value++;
    currentQuestion.value = questions[currentIndex.value];
    
  }

  if(currentIndex.value === (questions.length - 1 )){
    situacao.value=true;
  }

}
</script>

<style scoped>
.quiz-container {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}

.title {
  font-size: 24px;
  margin-bottom: 20px;
}

.start-button {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.question {
  font-size: 20px;
  margin-bottom: 20px;
}

.options {
  list-style-type: none;
  padding: 0;
}

.option-button {
  padding: 10px 20px;
  margin: 5px;
  font-size: 16px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
}

.option-button:hover {
  background-color: #ccc;
}

.score {
  font-size: 18px;
  margin-top: 20px;
}

.remaining {
  font-size: 16px;
  margin-top: 10px;
}

.finished {
  font-size: 20px;
  margin-top: 20px;
  color: green;
}
</style>



