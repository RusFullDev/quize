<script setup>
import { toRefs, ref, onMounted, onUnmounted } from "vue";
import { Icon } from "@iconify/vue";

const props = defineProps({
  modelValue: Boolean,
  msg: Array,
});

const emit = defineEmits(["update:modelValue"]);

const { modelValue, msg } = toRefs(props);

const toogleSide = () => {
  emit("update:modelValue", !modelValue.value);
};

const hour = ref("01:00:00");
let intervalId;

const startTimer = () => {
  let duration = 60 * 60; // 1 hour in seconds

  intervalId = setInterval(() => {
    const hours = Math.floor(duration / 3600);
    const minutes = Math.floor((duration % 3600) / 60);
    const seconds = duration % 60;

    hour.value = `${String(hours).padStart(2, "0")}:${String(minutes).padStart(
      2,
      "0"
    )}:${String(seconds).padStart(2, "0")}`;

    if (duration > 0) {
      duration--;
    } else {
      clearInterval(intervalId);
    }
  }, 1000);
};

onMounted(() => {
  startTimer();
});

onUnmounted(() => {
  clearInterval(intervalId);
});
</script>

<template>
  <div
    :class="modelValue ? 'w-[45%]' : 'w-[65px]'"
    class="fixed top-0 right-0 h-screen duration-300 bg-primary"
  >
    <div class="flex items-center justify-between shadow-md pr-5">
      <div>
        <button
          class="bg-orange-700 py-8 px-2 flex justify-center hover:bg-slate-900 hover:text-white"
          :class="!modelValue ? 'w-[65px]' : ''"
          @click="toogleSide"
        >
          <Icon
            :class="!modelValue ? '' : 'rotate-180'"
            class="text-4xl"
            icon="iconamoon:arrow-left-2-bold"
          />
        </button>
      </div>

      <div class="text-3xl py-8">{{ hour }}</div>

      <button
        class="flex items-center text-2xl font-light text-[#8b8b8b] underline hover:text-blue-700"
        icon="iconamoon:arrow-left-2-bold"
      >
        <a href="#">Chiqish</a>
        <Icon class="text-xl" icon="iconamoon:arrow-right-2-bold" />
      </button>
    </div>
    <div
      :class="!modelValue ? 'hidden' : block"
      class="py-3 px-4 text-base items-center"
    >
      <p class="py-3">Matematika</p>
      <p class="border-t-2 border-dotted w-[90%] border-[#eb885d] py-2"></p>
      <a
        class="p-2 px-3 rounded-full border-[1.5px] border-black m-1 hover:bg-black hover:text-white"
        v-for="(item, index) in 5"
        :class="msg[index].userAnswer ? 'bg-black text-white' : ''"
        :href="`#${item}`"
        :key="index"
      >
        {{ item }}
      </a>
      <p></p>
    </div>
  </div>
</template>

<style scoped></style>
