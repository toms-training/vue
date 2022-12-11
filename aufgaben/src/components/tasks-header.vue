<template>
    <h1 class="text-center" v-bind:style="titleStyle">{{ title }}</h1>
    <button class="btn btn-primary" v-bind:disabled="!canAddTask" @click="addTask">Aufgabe hinzufügen</button>
    <hr>
</template>
<script>
export default {
    name: 'TasksHeader',
    data() {
        return {
            title: 'Aufgabenliste',
            canAddTask: true,
            titleStyle: {
                color: 'cadetblue',
                margin: '10px auto 25px',
                fontFamily: '"Segoe UI", Tahoma, Geneva, Verdana, sans-serif',
                fontWeight: 600
            }
        }
    },
    methods: {
        addTask() { }
    }
}
</script>