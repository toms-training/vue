<template>
  <section class="container px-6 py-16 prose">
    <div id="tasks-header">
      <h1 class="text-center" v-bind:style="titleStyle">{{ title }}</h1>
      <button class="btn btn-primary" v-bind:disabled="!canAddTask" @click="addTask">Aufgabe hinzufügen</button>
      <hr>
    </div>
    <div id="tasks-list" v-if="!showForm">
      <section>
        <section id="no-tasks" v-if="tasks.length === 0">Keine Aufgaben verfügbar</section>
        <section v-for="task in tasks" :key="task.id" v-bind:id="`task-${task.id}`" class="card border-2 m-4">
          <div class="card-body">
            <h2 class="card-title" v-bind:class="{'task-done': task.done}">{{task.description}}</h2>
            <div class="card-actions justify-end">
              <button class="btn btn-primary" @click="editTask(task)">Bearbeiten</button>
              <button class="btn btn-error" v-on:click="deleteTask(task.id)">Löschen</button>
            </div>
          </div>
        </section>
      </section>
    </div>
    <div id="task-form" v-if="showForm">
      <section>
        <h2>Aufgabe {{ currentTask.id > 0 ? 'bearbeiten' : 'hinzufügen' }}</h2>
        <form @submit.prevent="submitTask">
          <div class="form-control px-4 my-5">
            <input type="text" class="input input-bordered" id="description" placeholder="Beschreibung"
              v-model.trim="currentTask.description" :class="{error: descriptionIsEmpty}">
          </div>
          <label class="cursor-pointer px-4 label justify-start">
            <div class="mr-2">
              <input type="checkbox" id="task-done" v-model="currentTask.done" value="true" class="checkbox" />
              <span class="checkbox-mark" for="task-done"></span>
            </div>
            <span class="label-text">Erledigt</span>
          </label>
          <div class="form-control px-4 my-5">
            <input class="btn btn-sm btn-primary" type="submit"
              :value="currentTask.id > 0 ? 'Aktualisieren' : 'Hinzufügen'" :disabled="!activateSubmit">
          </div>
        </form>
      </section>
    </div>
  </section>
</template>

<script>
import { Task } from './entities/task.class';

export default {
  name: 'App',
  data() {
    return {
      title: 'Aufgabenliste',
      canAddTask: true,
      tasks: createTasks(3),
      titleStyle: {
        color: 'cadetblue',
        margin: '10px auto 25px',
        fontFamily: '"Segoe UI", Tahoma, Geneva, Verdana, sans-serif',
        fontWeight: 600
      },
      currentTask: new Task(''),
      showForm: false,
      descriptionIsEmpty: false,
      activateSubmit: false
    }
  },
  methods: {
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
    submitTask() {
      const isNew = this.currentTask.id === 0;

      if (isNew) {
        this.currentTask.id = generateId(this.tasks);
        this.tasks.push(this.currentTask);
      } else {
        this.tasks = this.tasks.map(task => {
          if (task.id !== this.currentTask.id) return task;

          return this.currentTask;
        });
      }

      this.tasks = sortTasksByDone(this.tasks);
      this.currentTask = new Task('');
      this.showForm = false;
    },
    addTask() {
      this.showForm = true;
    },
    editTask(task) {
      this.currentTask = task;
      this.showForm = true;
    }
  },
  watch: {
    currentTask: {
      handler(task) {
        this.descriptionIsEmpty = task.description?.length === 0;
        this.activateSubmit = !this.descriptionIsEmpty;
      },
      deep: true,
      immediate: true
    }
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

function generateId(tasks) {
  return tasks.reduce(
    (previousId, task) => previousId > task.id ? previousId : task.id + 1,
    0
  );
}

</script>

<style scoped>
.hidden {
  display: none;
}
.task-done {
  text-decoration: line-through;
}
.error {
  border-color: red;
}
</style>
