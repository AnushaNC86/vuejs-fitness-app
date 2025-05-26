<script setup>
import Welcome from "./components/pages/Welcome.vue";
import Layout from "./components/layouts/Layout.vue";
import Dashboard from "./components/pages/Dashboard.vue";
import Workout from "./components/pages/Workout.vue";
import { computed, onMounted, ref } from "vue";
import { workoutProgram } from "./utils";

const defaultData = {};
for (let workoutIdx in workoutProgram) {
  const workoutData = workoutProgram[workoutIdx];
  //create a for loop where we iterate over every workout
  defaultData[workoutIdx] = {}; // initialize the workout data obj

  // nested loop to loop over every exercise within a workout, and initialize it's input value to an empty string
  for (let e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = "";
  }
}

const selectedDisplay = ref(1); // 1: Welcome, 2: Dashboard, 3: Workout
const data = ref(defaultData);
const selectedWorkout = ref(-1);

const isWorkoutCompleted = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value];
  if (!currWorkout) {
    return false;
  } // guard clause to exit function

  const isCompleteCheck = Object.values(currWorkout).every((ex) => !!ex);
  return isCompleteCheck;
});

const firstIncompleteWorkoutIndex = computed(() => {
  const allWorkouts = data.value;
  if (!allWorkouts) {
    return -1;
  }

  // loop over every key value pair, and check if the workout is complete or not
  for (const [index, workout] of Object.entries(allWorkouts)) {
    const isComplete = Object.values(workout).every((ex) => !!ex);
    if (!isComplete) {
      return parseInt(index);
    }
  }
  return -1; // all are complete
});

const handleChangeDisplay = (idx) => {
  selectedDisplay.value = idx;
};

const handleSelectWorkout = (idx) => {
  selectedWorkout.value = idx;
  selectedDisplay.value = 3; // Switch to Workout display
};

function handleSaveWorkout() {
  // save the current data snapshot to localstorage so that we can retrieve it next time
  localStorage.setItem("workouts", JSON.stringify(data.value));

  // show the dashboard
  selectedDisplay.value = 2;

  // deselect a workout
  selectedWorkout.value = -1;
}

function handleResetPlan() {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage.removeItem("workouts");
}

onMounted(() => {
  if (!localStorage) {
    return;
  }
  if (localStorage.getItem("workouts")) {
    // only enter the if block if we find some data saved to the key workouts in localstroage database
    const savedData = JSON.parse(localStorage.getItem("workouts"));
    data.value = savedData;
    selectedDisplay.value = 2; // if they have data, then we dont want them landing on the welcome screen every time they enter the app
  }
});
</script>

<template>
  <Layout>
    <Welcome
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay === 1"
    />
    <Dashboard
      :handleSelectWorkout="handleSelectWorkout"
      :handleResetPlan="handleResetPlan"
      :firstIncompleteWorkoutIndex="firstIncompleteWorkoutIndex"
      v-if="selectedDisplay === 2"
    />
    <Workout
      :data="data"
      :selectedWorkout="selectedWorkout"
      :isWorkoutCompleted="isWorkoutCompleted"
      :handleSaveWorkout="handleSaveWorkout"
      v-if="workoutProgram?.[selectedWorkout]"
    />
  </Layout>
</template>

<style scoped></style>
