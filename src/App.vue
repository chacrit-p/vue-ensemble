<script setup>
import "axios";
import { ref, onMounted } from "vue";

const BASE_URL = import.meta.env.VITE_BASE_API_URL;

const predictState = ref(null);
const loadingState = ref(false);
const formState = ref({
  gender: 0,
  age: 0,
});

const predict = () => {
  loadingState.value = true;
  axios
    .post(`${BASE_URL}/predict`, {})
    .then((response) => {
      console.log(response.data);
    })
    .catch((error) => {
      console.error(error);
    })
    .finally(() => {
      loadingState.value = false;
    });
};
</script>

<template>
  <div class="p-5 min-h-screen">
    <div class="max-w-2xl mx-auto">
      <h1 class="text-3xl font-bold text-center leading-loose mb-4">
        Student Depression Predict 😁🧑‍🎓
      </h1>
    </div>
  </div>
</template>
