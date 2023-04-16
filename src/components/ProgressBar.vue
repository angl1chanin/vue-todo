<template>
  <div class="infographics">
    <div class="infographics__progressbar">
      <span class="infographics__progress" :style="countProgress"></span>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProgressBar",
  props: ["tasks"],
  computed: {
    countProgress() {
      if (this.tasks) {
        let doneTasks = this.tasks.filter((item) => item.done).length
        return `width: ${doneTasks / this.tasks.length * 100}%`
      }
      return 0
    },
  },
};
</script>

<style>
.infographics__progressbar {
  position: relative;
  width: 100%;
  margin: 30px 0;
  height: 10px;
  border-radius: 5px;
  background: #d9d9d9;
  overflow: hidden;
}

.infographics__progress {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 0;
  background: #526bd0;
  transition: all 0.3s;
}
</style>