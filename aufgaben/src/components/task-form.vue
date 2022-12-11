<template>
    <h2>Aufgabe {{ currentTask.id > 0 ? 'bearbeiten' : 'hinzufügen' }}</h2>
    <form @submit.prevent="$emit('close', persistTask())">
        <div class="form-control px-4 my-5">
            <input type="text" class="input input-bordered" id="description" placeholder="Beschreibung"
                v-model.trim="currentTask.description" :class="{ error: descriptionIsEmpty }">
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
</template>

<script>
import { Task } from '../entities/task.class';
import axios from 'axios';
import apiConfig from '../api-config';

export default {
    name: 'TaskForm',
    created() {
        this.loadTasks();
    },
    props: {
        task: {
            type: Task,
            required: true
        }
    },
    data() {
        return {
            currentTask: this.task,
            descriptionIsEmpty: false,
            activateSubmit: false,
            tasks: []
        }
    },
    methods: {
        loadTasks() {
            axios.get(`${apiConfig.url}/tasks`)
                .then(response => this.tasks = response.data)
                .catch(error => console.error(error));
        },
        persistTask() {
            const url = `${apiConfig.url}/tasks`;

            if (this.currentTask.id === 0) {
                this.currentTask.id = generateId(this.tasks);
                return axios.post(url, this.currentTask);
            }

            return axios.put(`${url}/${this.currentTask.id}`, this.currentTask);
        }
    },
    emits: ['close'],
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

function generateId(tasks) {
    return tasks.reduce(
        (previousId, task) => previousId > task.id ? previousId : task.id + 1,
        1
    );
}
</script>

<style>
.error {
    border-color: red;
}
</style>