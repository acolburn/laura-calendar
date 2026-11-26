<script setup>
import { ref, watch } from "vue";
const days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"];
const defaultNotes = {
  Monday: "",
  Tuesday: "",
  Wednesday: "",
  Thursday: "",
  Friday: "",
};

function loadNotes() {
  const savedNotes = localStorage.getItem("laura-calendar");
  // if something's saved in localStorage, use those notes; if not, use the default blank notes
  return savedNotes ? JSON.parse(savedNotes) : defaultNotes;
}
const notes = ref(loadNotes());

watch(
  notes,
  function (newNotes) {
    localStorage.setItem("laura-calendar", JSON.stringify(newNotes));
  },
  { deep: true },
);

function clearWeek() {
  notes.value = { ...defaultNotes };
}
</script>

<template>
  <main class="container">
    <h1>Laura's Calendar</h1>
    <div class="week">
      <section v-for="day in days" :key="day">
        <h2>{{ day }}</h2>
        <textarea rows="4" v-model="notes[day]"></textarea>
      </section>
    </div>
    <button @click="clearWeek">Clear Week</button>
  </main>
</template>

<style scoped>
button {
  display: block;
  margin-left: auto;
}

@media (min-width: 1068px) {
  .week {
    display: flex;
    gap: 1rem;
  }

  .week section {
    flex: 1;
    /* min-width: 0; */
  }
}
</style>
