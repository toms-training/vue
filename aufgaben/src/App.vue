<template>
  <section class="container px-6 py-16 prose">
    <div id="tasks-header">
      <h1 class="text-center" v-bind:style="titleStyle">{{ title }}</h1>
      <button class="btn btn-primary" v-bind:disabled="!canAddTask">Aufgabe hinzufügen</button>
      <hr>
    </div>
    <div id="tasks-list">
      <section>
        <section id="no-tasks" v-if="tasks.length === 0">Keine Aufgaben verfügbar</section>
        <section v-for="task in tasks" :key="task.id" v-bind:id="`task-${task.id}`" class="card border-2 m-4">
          <div class="card-body">
            <h2 class="card-title" v-bind:class="{'task-done': task.done}">{{task.description}}</h2>
            <div class="card-actions justify-end">
              <button class="btn btn-primary">Bearbeiten</button>
              <button class="btn btn-error" v-on:click="deleteTask(task.id)">Löschen</button>
            </div>
          </div>
        </section>
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
      }
    }
  },
  methods: {
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
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

  return tasks.sort((a, b) => {
    if (a.done) return 1;
    if (b.done) return -1;

    return 0;
  });
}

</script>

<style scoped>
.hidden {
  display: none;
}
.task-done {
  text-decoration: line-through;
}
</style>
