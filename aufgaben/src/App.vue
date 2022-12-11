<template>
  <section class="container px-6 py-16 prose">
    <tasks-header></tasks-header>
    <tasks-list v-if="!showForm"></tasks-list>
    <task-form v-if="showForm" :task="activeTask"></task-form>
  </section>
</template>

<script>
import TasksHeader from "./components/tasks-header.vue";
import TasksList from "./components/tasks-list.vue";
import TaskForm from "./components/task-form.vue";
import { Task } from './entities/task.class';

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
      activeTask: new Task('')
    }
  },
  provide: {
    tasks: createTasks(3)
  }
}

function createTasks(amount) {
  const tasks = [];

  for (let i = 1; i <= amount; i++) {
    const task = new Task(`Aufgabe ${i}`);
    task.id = i;

    if (i % 2 === 0) task.done = true;

    tasks.push(task);
  }

  return sortTasksByDone(tasks);
}

function sortTasksByDone(tasks) {
  return tasks.sort((a, b) => {
    if (a.done) return 1;
    if (b.done) return -1;

    return 0;
  });
}

/*function generateId(tasks) {
  return tasks.reduce(
    (previousId, task) => previousId > task.id ? previousId : task.id + 1,
    0
  );
}*/

</script>
