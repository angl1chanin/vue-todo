<template>
  <div>
    <ul class="tasks" v-if="tasks">
      <ProgressBar v-show="tasks.length > 0" :tasks="tasks" />
      <li class="tasks__item" v-for="task in sortedTasks" :key="task.id">
        <SingleTask @updateTask="updateTasks" :task="task" />
      </li>
    </ul>
    <div v-else class="loading">
      <div class="letter-holder">
        <div class="l-1 letter">L</div>
        <div class="l-2 letter">o</div>
        <div class="l-3 letter">a</div>
        <div class="l-4 letter">d</div>
        <div class="l-5 letter">i</div>
        <div class="l-6 letter">n</div>
        <div class="l-7 letter">g</div>
        <div class="l-8 letter">.</div>
        <div class="l-9 letter">.</div>
        <div class="l-10 letter">.</div>
      </div>
    </div>
  </div>
</template>

<script>
import ProgressBar from './ProgressBar.vue';
import SingleTask from "./SingleTask.vue";

export default {
  name: "TaskList",
  props: ["tasks"],
  components: {
    SingleTask, ProgressBar,
  },
  methods: {
    updateTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  },
  computed: {
    countDoneTasks() {
      return this.tasks.filter((item) => item.done).length;
    },
    sortedTasks() {
      return this.tasks.sort((a, b) => (a.done === b.done) ? 0 : a.done ? 1 : -1);
    }
  }
};
</script>

<style>
.tasks__item:not(:last-child) {
  margin-bottom: 20px;
}
.loading {
  display: flex;
  justify-content: center;
}
.letter-holder {
  padding: 16px;
}
.letter {
  float: left;
  font-size: 16px;
  color: #777;
  animation-name: loadingF;
  animation-duration: 1.6s;
  animation-iteration-count: infinite;
  animation-direction: linear;
}
.l-1 {
  animation-delay: 0.48s;
}
.l-2 {
  animation-delay: 0.6s;
}
.l-3 {
  animation-delay: 0.72s;
}
.l-4 {
  animation-delay: 0.84s;
}
.l-5 {
  animation-delay: 0.96s;
}
.l-6 {
  animation-delay: 1.08s;
}
.l-7 {
  animation-delay: 1.2s;
}
.l-8 {
  animation-delay: 1.32s;
}
.l-9 {
  animation-delay: 1.44s;
}
.l-10 {
  animation-delay: 1.56s;
}
@keyframes loadingF {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>