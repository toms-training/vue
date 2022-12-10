<template>
  <section class="container px-6 py-16 prose">
    <div id="tasks-header">
      <h1 class="text-center" v-bind:style="titleStyle">{{ title }}</h1>
      <button class="btn btn-primary" v-bind:disabled="!canAddTask">Aufgabe hinzufügen</button>
      <hr>
    </div>
    <div id="tasks-list">
      <section>
        <section id="no-tasks" v-bind:class="{hidden: tasks.length > 0}">Keine Aufgaben verfügbar</section>
      </section>
    </div>
  </section>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      title: 'Aufgabenliste',
      canAddTask: true,
      tasks: [],
      titleStyle: {
        color: 'cadetblue',
        margin: '10px auto 25px',
        fontFamily: '"Segoe UI", Tahoma, Geneva, Verdana, sans-serif',
        fontWeight: 600
      }
    }
  }
}
</script>

<style scoped>
.hidden {
  display: none;
}
</style>
