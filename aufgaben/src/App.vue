<template>
  <section class="container px-6 py-16 prose">
    <div id="tasks-header">
      <h1 class="text-center">{{ title }}</h1>
      <button class="btn btn-primary" v-bind:disabled="!canAddTask">Aufgabe hinzufügen</button>
      <hr>
    </div>
    <div id="tasks-list">
      <section>
        <section id="no-tasks">Keine Aufgaben verfügbar</section>
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
      canAddTask: true
    }
  }
}
</script>
