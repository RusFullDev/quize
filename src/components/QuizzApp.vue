<template>
  <div>
    <div
      v-if="isFinishTest"
      @click="finishTest"
      class="fixed z-10 w-full bg-black/80 h-screen duration-300 delay-0"
    ></div>

    <div v-if="isFinishTest" class="z-10 fixed top-[20%] left-1/3 duration-300">
      <div
        class="text-white bg-slate-500 p-6 px-20 flex flex-col gap-4 items-center"
      >
        <h1>To'g'ri javoblar soni : {{ trueQuizzes }}</h1>
        <h1>Umumiy foiz : {{ percentQuizzes }} %</h1>
        <h1>Umumiy ball : {{ userBall }} / 100</h1>

        <circle-progress :percent="percentQuizzes" />
        <button
          @click="finishTest"
          :class="!isFinishTest ? 'bg-green-500 text-white' : 'bg-primary'"
          class="py-3 px-5 rounded"
        >
          {{ isFinishTest ? "Qayta topshirish" : "Testni yakunlash" }}
        </button>
      </div>
    </div>

    <div
      :class="isSideMenu ? 'w-[calc(100%-45%)]' : 'w-[calc(100%-65px)]'"
      class="shadow-md duration-300"
    >
      <h1 class="py-8 text-3xl text-center">Matematika</h1>
    </div>

    <div
      class="m-10 duration-300"
      :class="isSideMenu ? 'w-[calc(100%-50%)] ' : 'w-[1000px] mx-auto '"
    >
      <QuizzItem
        v-for="(quizz, index) in quizzes"
        :quizz="quizz"
        @user-select-answer="handleSelectAnswer"
      />
    </div>

    <div
      :class="isSideMenu ? 'w-[calc(100%-45%)]' : 'w-[calc(100%-65px)]'"
      class="bg-black duration-300 p-24 justify-center"
    >
      <div class="flex justify-center">
        <button
          @click="finishTest"
          :class="!isFinishTest ? 'bg-green-500 text-white' : 'hidden'"
          class="py-3 px-5 rounded"
        >
          {{ isFinishTest ? "Qayta topshirish" : "Testni yakunlash" }}
        </button>
      </div>
    </div>

    <QuizzSider v-model="isSideMenu" :msg="quizzes" />
  </div>
</template>

<script setup>
import "vue3-circle-progress/dist/circle-progress.css";
import CircleProgress from "vue3-circle-progress";
import { ref, reactive } from "vue";
import QuizzSider from "./QuizzSider.vue";
import QuizzItem from "./QuizzItem.vue";


 const quizzes = reactive([
  {
    id: 1,
    question: "2+2 = ?",
    answers: ["4", "2", "3", "5"],
    correctAnswer: "4",
  },
  {
    id: 2,
    question: "2+12 = ?",
    answers: ["14", "24", "13", "25"],
    correctAnswer: "14",
  },
  {
    id: 3,
    question: "2+6 = ?",
    answers: ["4", "2", "8", "3"],
    correctAnswer: "8",
  },
  {
    id: 4,
    question: "5+2 = ?",
    answers: ["4", "7", "3", "2"],
    correctAnswer: "7",
  },
  {
    id: 5,
    question: "1+2 = ?",
    answers: ["4", "2", "3", "5"],
    correctAnswer: "4",
  },
]);

const isSideMenu = ref(false);



const refresh = () => {};

const handleSelectAnswer = (obj) => {
  quizzes.forEach((item) => {
    if (item.id == obj.id) {
      item.userAnswer = obj.userAnswer;
    }
  });
};

const isFinishTest = ref(false);
const trueQuizzes = ref(0);
const percentQuizzes = ref(0);
const userBall = ref(0);

const finishTest = () => {
  if (isFinishTest.value) window.location.reload();

  isFinishTest.value = true;

  quizzes.forEach((item) => {
    if (item.correctAnswer == item.userAnswer) {
      trueQuizzes.value++;
    }
  });

  percentQuizzes.value = (trueQuizzes.value * 100) / quizzes.length;
  userBall.value = percentQuizzes.value;
};
</script>

<style lang="scss" scoped></style>
