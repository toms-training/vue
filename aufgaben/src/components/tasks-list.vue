<template>
    <section id="no-tasks" v-if="tasks.length === 0">Keine Aufgaben verfügbar</section>
    <section v-for="task in tasks" :key="task.id" v-bind:id="`task-${task.id}`" class="card border-2 m-4">
        <div class="card-body">
            <h2 class="card-title" v-bind:class="{ 'task-done': task.done }">{{ task.description }}</h2>
            <div class="card-actions justify-end">
                <button class="btn btn-primary" @click="editTask(task)">Bearbeiten</button>
                <button class="btn btn-error" v-on:click="deleteTask(task.id)">Löschen</button>
            </div>
        </div>
    </section>
</template>

<script>
import { Task } from '../entities/task.class';

export default {
    name: 'TasksList',
    data() {
        return {
            tasks: createTasks(3)
        }
    },
    methods: {
        deleteTask(id) {
            this.tasks = this.tasks.filter(task => task.id !== id);
        },
        editTask() { }
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
</script>

<style>
.task-done {
    text-decoration: line-through;
}
</style>