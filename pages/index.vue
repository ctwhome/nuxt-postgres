<template>
  <div>
    <v-container>
      <Logo />
      <p>Nuxt - Vite - Postgres - PostgREST</p>
    </v-container>
    <v-container>
      <v-row class="d-flex my-5">
        <v-btn href="http://localhost:4000" target="_blank"
          >API Schema - Swagger</v-btn
        >
        <v-btn href="https://postgrest.org/" target="_blank" class="ml-4"
          >postgREST</v-btn
        >
      </v-row>
    </v-container>
    <v-container>
      <v-row>
        <v-text-field
          label="Add todo"
          hide-details
          outlined
          v-model="todo"
          clearable
          @keydown.enter="
            todo && addTodo(todo)
            todo = ''
          "
        ></v-text-field>
        <v-btn
          :disabled="!todo"
          @click="
            todo && addTodo(todo)
            todo = ''
          "
          >Add todo</v-btn
        >
      </v-row>
      <v-card class="mx-auto mt-10" max-width="400" tile>
        <v-card-title>Todos</v-card-title>
        <div v-if="todos.length > 0">
          <v-list-item v-for="(todo, index) in todos" :key="index">
            <v-list-item-content>
              {{ todo.text }}
            </v-list-item-content>
            <v-list-item-action @click="removeTodo(todo.id)">
              <v-btn icon>
                <v-icon color="grey lighten-1">mdi-close</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </div>
      </v-card>
    </v-container>
  </div>
</template>

<script lang="ts">
// import { supabase } from '~/plugins/supabase'
interface Todo {
  id: integer
  text: string
}
export default {
  async asyncData() {
    const response = await fetch('http://localhost:8080/todos')
    const todos = await response.json()
    return { todos }
  },

  data() {
    return {
      todo: '',
      todos: [],
    }
  },
  methods: {
    async addTodo(text: string) {
      try {
        const response = await fetch('http://localhost:8080/todos', {
          method: 'POST',
          headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            text,
          }),
        })
      } catch (e) {
        console.error(e)
      } finally {
        await this.$nuxt.refresh()
      }
    },

    async removeTodo(id: number): void {
      try {
        const response = await fetch(
          'http://localhost:8080/todos?id=eq.' + id,
          {
            method: 'DELETE',
          }
        )
      } catch (e) {
        console.error(e)
      } finally {
        await this.$nuxt.refresh()
      }
    },
  },
}
</script>
