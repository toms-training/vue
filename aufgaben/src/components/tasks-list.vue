<template>
    <section id="no-tasks" v-if="tasks.length === 0">Keine Aufgaben verfügbar</section>
    <section v-for="task in tasks" :key="task.id" v-bind:id="`task-${task.id}`" class="card border-2 m-4">
        <div class="card-body">
            <h2 class="card-title" v-bind:class="{ 'task-done': task.done }">{{ task.description }}</h2>
            <div class="card-actions justify-end">
                <button class="btn btn-primary" @click="$emit('edit', task)">Bearbeiten</button>
                <button class="btn btn-error" v-on:click="$emit('delete', task)">Löschen</button>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: 'TasksList',
    props: {
        tasks: {
            type: Array,
            default: () => []
        }
    },
    emits: ['edit', 'delete']
}
</script>

<style>
.task-done {
    text-decoration: line-through;
}
</style>