<template>
  <section class="container px-6 py-16 prose">
    <tasks-header title="Aufgabenliste" :titleStyle="titleStyle" @addTask="addTask"></tasks-header>
    <tasks-list v-if="!showForm" @edit="editTask" @delete="deleteTask" :tasks="tasks"></tasks-list>
    <task-form v-if="showForm" :task="activeTask" @submit="submitTask"></task-form>
  </section>
</template>

<script>
import TasksHeader from "./components/tasks-header.vue";
import TasksList from "./components/tasks-list.vue";
import TaskForm from "./components/task-form.vue";
import { Task } from './entities/task.class';
import { TitleStyle } from "./entities/title-style.class";

export default {
  name: 'App',
  components: {
    TasksHeader,
    TasksList,
    TaskForm
  },
  data() {
    return {
      showForm: false,
      activeTask: new Task(''),
      titleStyle: createTitleStyle(),
      tasks: []
    }
  },
  methods: {
    addTask() {
      this.activeTask = new Task('');
      this.showForm = true;
    },
    editTask(task) {
      this.activeTask = task;
      this.showForm = true;
    },
    deleteTask(task) {
      this.tasks = this.tasks.filter(next => next.id !== task.id);
      this.sortTasksByDone();
    },
    submitTask(task) {
      if (task.id > 0) this.updateTask(task);
      else this.createTask(task);

      this.sortTasksByDone();
      this.showForm = false;
    },
    createTask(task) {
      task.id = generateId(this.tasks);

      this.tasks.push(task);
    },
    updateTask(task) {
      this.tasks = this.tasks.map(next => {
        if (next.id === task.id) return task;

        return next;
      });
    },
    sortTasksByDone() {
      this.tasks = this.tasks.sort((a, b) => {
        if (a.done) return 1;
        if (b.done) return -1;

        return 0;
      });
    }
  }
}

function createTitleStyle() {
  const style = new TitleStyle();
  style.color = 'cadetblue';
  style.fontFamily = '"Segoe UI", Tahoma, Geneva, Verdana, sans-serif';
  style.fontWeight = 600;

  return style;
}

function generateId(tasks) {
  return tasks.reduce(
    (previousId, task) => previousId > task.id ? previousId : task.id + 1,
    1
  );
}

</script>
