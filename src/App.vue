<template>
  <div id="app">
    <h1>Pomodoro Timer</h1>
    <PomodoroTimer />
  </div>
</template>

<script>
import PomodoroTimer from "./components/PomodoroTimer.vue";

export default {
  name: "App",
  components: {
    PomodoroTimer,
  },
  data() {
    return {
      sessionCount: 0,
    };
  },
};
</script>