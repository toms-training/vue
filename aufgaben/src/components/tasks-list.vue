<template>
    <section id="no-tasks" v-if="tasks.length === 0">Keine Aufgaben verfügbar</section>
    <section v-for="task in tasks" :key="task.id" v-bind:id="`task-${task.id}`" class="card border-2 m-4">
        <div class="card-body">
            <h2 class="card-title" v-bind:class="{ 'task-done': task.done }">{{ task.description }}</h2>
            <div class="card-actions justify-end">
                <button class="btn btn-primary" @click="$emit('edit', task)">Bearbeiten</button>
                <button class="btn btn-error" v-on:click="deleteTask(task)">Löschen</button>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios';
import apiConfig from '../api-config';
import { Task } from "../entities/task.class";

export default {
    name: 'TasksList',
    created() {
        this.loadTasks();
    },
    data() {
        return {
            tasks: []
        }
    },
    methods: {
        loadTasks() {
            axios.get(`${apiConfig.url}/tasks`)
                .then(response => {
                    const data = response.data;

                    if (!Array.isArray(data) || data.length === 0) {
                        this.tasks = [];
                        return;
                    }

                    this.tasks = data.map(item => Object.assign(new Task(''), item));
                    this.sortTasksByDone();
                })
                .catch(error => console.error(error));
        },
        sortTasksByDone() {
            this.tasks = this.tasks.sort((a, b) => {
                if (a.done) return 1;
                if (b.done) return -1;

                return 0;
            });
        },
        deleteTask(task) {
            axios.delete(`${apiConfig.url}/tasks/${task.id}`)
                .then(() => this.loadTasks())
                .catch(error => console.error(error));
        }
    },
    emits: ['edit']
}
</script>

<style>
.task-done {
    text-decoration: line-through;
}
</style>