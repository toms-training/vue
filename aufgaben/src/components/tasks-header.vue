<template>
    <h1 class="text-center" :style="style">{{ title }}</h1>
    <button class="btn btn-primary" :disabled="!canAddTask" @click="$emit('addTask')">Aufgabe hinzufügen</button>
    <hr>
</template>
<script>
import { TitleStyle } from "../entities/title-style.class";

export default {
    name: 'TasksHeader',
    props: {
        title: {
            type: String,
            required: true
        },
        titleStyle: {
            type: TitleStyle,
            required: false
        },
        canAddTask: {
            type: Boolean,
            required: false,
            default: true
        }
    },
    data() {
        return {
            style: {
                ...this.titleStyle,
                margin: '10px auto 25px',
            }
        }
    },
    emits: ['addTask']
}
</script>