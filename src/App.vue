<template>
  <div class="todo">
    <div class="todo__container container">
      <div class="todo__nav">
        <p class="todo__today">{{ getDate }}</p>
        <div class="todo__add">
          <form @submit.prevent="addTask">
            <input
              type="text"
              class="todo__add-input"
              :class="{ 'todo__add-input_open': showInput }"
              v-model="taskTitle"
              placeholder="Add task"
            />
            <button @click="handleClick" class="todo__add-btn" type="button">
              <span></span>
            </button>
          </form>
        </div>
      </div>
      <p v-if="tasks.length === 0" class="todo__number-of-tasks">Add your first task!</p>
      <p v-else-if="tasksLength === 0" class="todo__number-of-tasks">Congrats! All tasks are done</p>
      <p v-else class="todo__number-of-tasks">{{ tasksLength }} tasks</p>
      <div v-if="tasks" class="todo__tasks">
        <TaskList :tasks="tasks" @removeTask="removeTask" @confirmEditing="confirmEditing" />
      </div>
    </div>
  </div>
</template>

<script>
import TaskList from "./components/TaskList.vue";
import SingleTask from "./components/SingleTask.vue";

export default {
  name: "App",
  components: {
    TaskList,
    SingleTask,
  },
  data() {
    return {
      tasks: [],
      showInput: false,
      taskTitle: "",
    };
  },
  methods: {
    updateLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    handleClick() {
      if (this.taskTitle) {
        this.addTask()
      } else {
        this.showInput = !this.showInput;
      }
    },
    getTime() {
      Date.prototype.timeNow = function () {
        return (
          (this.getHours() < 10 ? "0" : "") +
          this.getHours() +
          ":" +
          (this.getMinutes() < 10 ? "0" : "") +
          this.getMinutes()
        );
      };
      let newDate = new Date();
      return newDate.timeNow()
    },
    addTask() {
      if (this.tasks && this.taskTitle) {
        this.tasks.push({
          id: Math.random(200),
          title: this.taskTitle,
          time: this.getTime(),
          done: false,
        });
        this.updateLocalStorage()
        this.taskTitle = ""
      }
    },
    removeTask(task) {
      this.tasks = this.tasks.filter(item => item !== task)
      this.updateLocalStorage()
    },
    confirmEditing(task, newTitle) {
      this.tasks = this.tasks.map(item => {
        if (item.id === task.id) {
          item.title = newTitle
        }
        return item
      })
      this.updateLocalStorage()
    }
  },
  computed: {
    getDate() {
      const weekday = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const month = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ];
      const d = new Date();
      return `${weekday[d.getDay()]}, ${d.getDate()} ${month[d.getMonth()]}`;
    },
    tasksLength() {
      return this.tasks.filter((task) => task.done === false).length;
    },
  },
  created() {
    let tasksFromLS = localStorage.getItem('tasks')
    if (tasksFromLS) {
      this.tasks = JSON.parse(tasksFromLS)
    }
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

ul,
li {
  list-style-type: none;
}

body {
  background-color: #526bd0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  max-width: 1140px;
  padding: 0 30px;
  margin: 0 auto;
}

.todo__container {
  background-color: #f3f3f3;
  padding: 30px;
  max-width: 400px;
  border-radius: 15px;
}

.todo__nav {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin-bottom: 15px;
}

.todo__add-input {
  outline: none;
  border: none;
  border-bottom: 1px solid #464850;
  background-color: transparent;
  font-size: 16px;
  font-family: inherit;
  width: 0;
  transition: width 0.2s linear;
  color: #464850;
}

.todo__add-input_open {
  width: 130px;
}

.todo__add-btn {
  border: none;
  background-color: #526bd0;
  width: 40px;
  height: 40px;
  border-radius: 20px;
  font-size: 28px;
  font-weight: 300;
  color: #fff;
  position: relative;
  cursor: pointer;
  transition: transform 0.2s linear;
}

.todo__add-btn:hover {
  transform: rotate(25deg);
}

.todo__add-btn span::after {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  height: 2px;
  width: 15px;
}

.todo__add-btn span::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  width: 2px;
  height: 15px;
}

.todo__today {
  color: #464850;
  font-size: 20px;
  font-weight: 600;
}

.todo__number-of-tasks {
  color: #526bd0;
  font-weight: 600;
  text-align: left;
  margin-bottom: 30px;
}
</style>
