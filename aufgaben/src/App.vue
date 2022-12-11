<template>
  <section class="container px-6 py-16 prose">
    <tasks-header></tasks-header>
    <tasks-list v-if="!showForm"></tasks-list>
    <task-form v-if="showForm"></task-form>
  </section>
</template>

<script>
import TasksHeader from "./components/tasks-header.vue";
import TasksList from "./components/tasks-list.vue";
import TaskForm from "./components/task-form.vue";

export default {
  name: 'App',
  components: {
    TasksHeader,
    TasksList,
    TaskForm
  },
  data() {
    return {
      showForm: false
    }
  }
}

/*function generateId(tasks) {
  return tasks.reduce(
    (previousId, task) => previousId > task.id ? previousId : task.id + 1,
    0
  );
}*/

</script>
