<script setup>
import { computed, onMounted, ref } from 'vue'
import Layout from './components/layouts/Layout.vue';
import Dashboard from './components/pages/Dashboard.vue';
import Welcome from './components/pages/Welcome.vue';
import Workout from './components/pages/Workout.vue';
import { workoutProgram } from './utils';

const defaultData = {};
for(let workoutIdx in workoutProgram) {
  defaultData[workoutIdx] = {};
  const workoutData = workoutProgram[workoutIdx];
  for (const e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = '';
  }
}
const selectedDisplay = ref(1);
const data = ref(defaultData);
const selectedWorkout = ref(-1);

const isWorkoutComplete = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value];
  if (!currWorkout) return false;

  const isCompleteCheck = Object.values(currWorkout).every(ex => !!ex);
  return isCompleteCheck;
})

const firstInCompleteWorkoutIndex = computed(() => {
  const allWorkouts = data.value;
  if (!allWorkouts) return -1;
  for (const [index, workout] of Object.entries(allWorkouts)) {
    const isComplete = Object.values(workout).every(ex => !!ex);
    if (!isComplete) return parseInt(index);
  }
  return -1;
})

function handleChangeDisplay(idx) {
  selectedDisplay.value = idx;
}

function handleSelectWorkout(idx) {
  selectedDisplay.value = 3;
  selectedWorkout.value = idx;
}

function handleSaveWorkout() {
  // save current data snapshot to localStorage so that we can retrieve it next time
  localStorage.setItem('workouts', JSON.stringify(data.value));
  //show the dashboard
  selectedDisplay.value = 2;
  // deselect a workout
  selectedWorkout.value = -1;
}

function handleResetPlan() {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage.removeItem('workouts');
}

function returnWelcomePage() {
  selectedDisplay.value = 1;
}

onMounted(() => {
  if(!localStorage) return;
  if(localStorage.getItem("workouts")) {
    data.value = JSON.parse(localStorage.getItem("workouts"));
    selectedDisplay.value = 2;
  }
})

</script>

<template>
  <Layout :returnWelcomePage="returnWelcomePage">
    <!-- Page 1 -->
    <Welcome :handleChangeDisplay="handleChangeDisplay" v-if="selectedDisplay == 1" />
    <!-- Page 2 -->
    <Dashboard
      :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex"  
      :handleSelectWorkout="handleSelectWorkout"
      :handleResetPlan="handleResetPlan"
      v-if="selectedDisplay == 2"
    />
    <!-- Page 3 -->
    <Workout 
      :isWorkoutComplete="isWorkoutComplete"
      :handleSaveWorkout="handleSaveWorkout"
      :data="data" 
      :selectedWorkout="selectedWorkout" 
      v-if="workoutProgram?.[selectedWorkout]"
    />
  </Layout>
</template>

<style scoped>

</style>
