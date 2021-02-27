<template>
  <div>
    <div class="container">
      <v-row>
        <Logo />
      </v-row>
      <v-card class="mx-auto mt-10" max-width="400" tile>
        <v-card-title>Todos</v-card-title>
        <v-list-item v-for="(todo, index) in todos" :key="index">
          <v-list-item-content>
            {{ todo.text }}
          </v-list-item-content>
        </v-list-item>
      </v-card>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      todos: [],
    }
  },
  async mounted() {
    const response = await fetch('http://localhost:8080/todos')
    const todos = await response.json() // .then((response) => response.json())
    this.todos = todos
  },
}
</script>
