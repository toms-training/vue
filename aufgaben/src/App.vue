<template>
  <section class="container px-6 py-16 prose">
    <tasks-header title="Aufgabenliste" :titleStyle="titleStyle" @addTask="addTask"></tasks-header>
    <tasks-list v-if="!showForm" @edit="editTask"></tasks-list>
    <task-form v-if="showForm" :task="activeTask" @close="closeForm"></task-form>
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
    closeForm(promise) {
      promise
        .then(() => this.showForm = false)
        .catch(error => console.error(error));
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

</script>
