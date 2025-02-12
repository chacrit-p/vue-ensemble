<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import InputError from "./components/InputError.vue";
import professions from "./data/professions.js";

const BASE_URL = import.meta.env.VITE_BASE_API_URL;

const predictState = ref(null);

const form = ref({
  gender: 0,
  age: null,
  profession: 0,
  academic_pressure: 0,
  work_pressure: 0,
  cga: 0,
  study_satisfaction: 0,
  job_satisfaction: 0,
  sleep_duration: 0,
  dietary_habits: 0,
  degree: 0,
  have_suicidal_thoughts: 0,
  work_study_hours: 0,
  financial_stress: 0,
  family_history_mental_illness: 0,
  loadingState: false,
  errors: [],
  error: "",
});

const predict = () => {
  form.loadingState = true;
  axios
    .post(`${BASE_URL}/predict`, form.value)
    .then((response) => {
      const data = response.data;
      predictState.value = data.prediction[0];
    })
    .catch((error) => {
      if (
        error.response &&
        error.response.data.message === "Validation Error"
      ) {
        form.value.errors = error.response.data.errors.reduce((acc, error) => {
          acc[error.loc[0]] = error.msg;
          return acc;
        }, {});
      } else {
        form.value.error = error.message;
        console.error("An error occurred:", error);
      }
    })
    .finally(() => {
      form.loadingState = false;
    });
};
</script>

<template>
  <div class="p-5 min-h-screen">
    <div class="max-w-2xl mx-auto">
      <div v-if="form.error" role="alert" class="alert alert-error my-5">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 shrink-0 stroke-current"
          fill="none"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>
        <span>Error! {{ form.error }}.</span>
      </div>
      <h1 class="text-3xl font-bold text-center leading-loose mb-4">
        Student Depression Predict 😁🧑‍🎓
      </h1>

      <form @submit.prevent="predict">
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Gender</legend>
          <select v-model="form.gender" class="select">
            <option value="0">Male</option>
            <option value="1">Female</option>
          </select>
          <InputError :message="form.errors.gender" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Enter Your age</legend>
          <input
            v-model="form.age"
            type="number"
            class="input"
            placeholder="22"
          />
          <InputError :message="form.errors.age" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Profession</legend>
          <select v-model="form.profession" class="select">
            <option
              v-for="(profession, key) in professions"
              :value="profession"
              :key="profession"
            >
              {{ key }}
            </option>
          </select>
          <InputError :message="form.errors.profession" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Academic Pressure</legend>
          <select v-model="form.academic_pressure" class="select">
            <option value="0">None</option>
            <option value="1">Very Low</option>
            <option value="2">Low</option>
            <option value="3">Moderate</option>
            <option value="4">High</option>
            <option value="5">Very High</option>
          </select>
          <InputError :message="form.errors.academic_pressure" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Work Pressure</legend>
          <select v-model="form.work_pressure" class="select">
            <option value="0">None</option>
            <option value="1">Very Low</option>
            <option value="2">Low</option>
            <option value="3">Moderate</option>
            <option value="4">High</option>
            <option value="5">Very High</option>
          </select>
          <InputError :message="form.errors.work_pressure" />
        </fieldset>
        <button
          :class="{ 'btn-disabled': form.loadingState }"
          class="btn btn-block btn-primary mt-4"
          type="submit"
        >
          Predict
        </button>
      </form>
    </div>
  </div>
</template>
