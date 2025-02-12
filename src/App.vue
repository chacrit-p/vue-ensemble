<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import InputError from "./components/InputError.vue";
import professions from "./data/professions.js";
import sleep_durations from "./data/sleep_durations.js";
import dietary_habits from "./data/dietary_habits.js";
import degrees from "./data/degrees.js";

const BASE_URL = import.meta.env.VITE_BASE_API_URL;

const predictState = ref(null);

const form = ref({
  gender: 0,
  age: null,
  profession: 0,
  academic_pressure: 0,
  work_pressure: 0,
  cga: null,
  study_satisfaction: 0,
  job_satisfaction: 0,
  sleep_duration: 0,
  dietary_habits: 0,
  degree: 0,
  have_suicidal_thoughts: 0,
  work_study_hours: 0,
  financial_stress: 1,
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
            placeholder="18-59"
            min="18"
            max="59"
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
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Enter Your CGA</legend>
          <input
            v-model="form.cga"
            type="number"
            class="input"
            placeholder="0-10"
            min="0"
            max="10"
          />
          <InputError :message="form.errors.cga" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Study Satisfaction</legend>
          <select v-model="form.study_satisfaction" class="select">
            <option value="0">None</option>
            <option value="1">Very Low</option>
            <option value="2">Low</option>
            <option value="3">Moderate</option>
            <option value="4">High</option>
            <option value="5">Very High</option>
          </select>
          <InputError :message="form.errors.study_satisfaction" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Job Satisfaction</legend>
          <select v-model="form.job_satisfaction" class="select">
            <option value="0">None</option>
            <option value="1">Very Low</option>
            <option value="2">Low</option>
            <option value="3">Moderate</option>
            <option value="4">High</option>
          </select>
          <InputError :message="form.errors.job_satisfaction" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Sleep Duration</legend>
          <select v-model="form.sleep_duration" class="select">
            <option
              v-for="(sleep_duration, key) in sleep_durations"
              :value="sleep_duration"
              :key="sleep_duration"
            >
              {{ key }}
            </option>
          </select>
          <InputError :message="form.errors.sleep_duration" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Dietary Habits</legend>
          <select v-model="form.dietary_habits" class="select">
            <option
              v-for="(dietary_habit, key) in dietary_habits"
              :value="dietary_habit"
              :key="dietary_habit"
            >
              {{ key }}
            </option>
          </select>
          <InputError :message="form.errors.dietary_habit" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Degree</legend>
          <select v-model="form.degree" class="select">
            <option
              v-for="(degree, key) in degrees"
              :value="degree"
              :key="degree"
            >
              {{ key }}
            </option>
          </select>
          <InputError :message="form.errors.degree" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Have Suicidal Thoughts</legend>
          <select v-model="form.have_suicidal_thoughts" class="select">
            <option value="0">No</option>
            <option value="1">Yes</option>
          </select>
          <InputError :message="form.errors.have_suicidal_thoughts" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Work/Study Hour</legend>
          <select v-model="form.work_study_hours" class="select">
            <option
              v-for="(hour, index) in Number(12)"
              :value="index"
              :key="hour"
            >
              {{ hour }} Hours
            </option>
          </select>
          <InputError :message="form.errors.work_study_hours" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">Select Fitness Stress</legend>
          <select v-model="form.financial_stress" class="select">
            <option value="1">Very Low</option>
            <option value="2">Low</option>
            <option value="3">Moderate</option>
            <option value="4">High</option>
            <option value="5">Very High</option>
          </select>
          <InputError :message="form.errors.financial_stress" />
        </fieldset>
        <fieldset class="fieldset">
          <legend class="fieldset-legend">
            Select Family History Mental Illness
          </legend>
          <select v-model="form.family_history_mental_illness" class="select">
            <option value="0">No</option>
            <option value="1">Yes</option>
          </select>
          <InputError :message="form.errors.family_history_mental_illness" />
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
