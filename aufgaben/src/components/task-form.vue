<template>
    <h2>Aufgabe {{ currentTask.id > 0 ? 'bearbeiten' : 'hinzufügen' }}</h2>
    <form @submit.prevent="submitTask">
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

export default {
    name: 'TaskForm',
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
            activateSubmit: false
        }
    },
    methods: {
        submitTask() { }
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
</script>

<style>
.error {
    border-color: red;
}
</style>