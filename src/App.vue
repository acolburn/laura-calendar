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
      <article v-for="day in days" :key="day">
        <h2>{{ day }}</h2>
        <textarea rows="5" v-model="notes[day]"></textarea>
      </article>
    </div>
    <button @click="clearWeek">Clear Week</button>
  </main>
</template>

<style scoped>
h1 {
  text-align: center;
  margin-block: 1.5rem 2rem;
}

h2 {
  margin-bottom: 0.75rem;
  font-size: 1.25rem;
}

.week article {
  box-shadow:
    0 2px 8px rgba(0, 0, 0, 0.1),
    0 1px 3px rgba(0, 0, 0, 0.08);
}

textarea {
  min-height: 12rem;
  padding: 0.75rem;
  font-size: 1.125rem;
  line-height: 1.6;
}

button {
  display: block;
  margin-left: auto;
  margin-top: 1.5rem;
  padding: 0.75rem 1.75rem;
  font-size: 1.125rem;
}

@media (min-width: 1068px) {
  .week {
    display: flex;
    gap: 1.25rem;
    align-items: stretch;
  }

  .week article {
    flex: 1;
    margin: 0;
    display: flex;
    flex-direction: column;
  }

  .week textarea {
    flex: 1;
    font-size: 0.95rem;
  }
}
</style>
