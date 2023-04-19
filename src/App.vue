<template>
  <div>
    <header>
      <h1>Vue Todo with TypeScript</h1>
    </header>
    <main>
      <TodoInput :item="todoText" v-model="todoText" @add="addTodoItem" />
      <div>
        <ul>
          <TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            :index="index"
            :todoItem="todoItem"
            @remove="removeTodos"
            @toggle="toggleTodoItemComplete"
          />
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import TodoInput from './components/TodoInput.vue';
import TodoListItem from './components/TodoListItem.vue';

const STORAGE_KEY = 'vue-todo-ts-v1';
const storage = {
  save(todos: Todo[]): void {
    const parsed = JSON.stringify(todos);
    localStorage.setItem(STORAGE_KEY, parsed);
  },

  fetch(): Todo[] {
    const todoItems = localStorage.getItem(STORAGE_KEY) || '[]';
    const result = JSON.parse(todoItems);
    return result;
  },
};

export interface Todo {
  title: string;
  done: boolean;
}

export default Vue.extend({
  components: { TodoInput, TodoListItem },
  data() {
    return {
      todoText: '',
      todoItems: [] as Todo[],
    };
  },
  methods: {
    addTodoItem(): void {
      const value = this.todoText;

      if (value !== '') {
        const todo: Todo = {
          title: value,
          done: false,
        };

        this.todoItems.push(todo);
        storage.save(this.todoItems);
        this.initTodoText();
      }
      return;
    },

    initTodoText(): void {
      this.todoText = '';
    },

    fetchTodoItems(): void {
      this.todoItems = storage.fetch().sort((a: Todo, b: Todo) => {
        if (a.title < b.title) {
          return -1;
        }
        if (a.title > b.title) {
          return 1;
        }
        return 0;
      });
    },

    removeTodos(index: number): void {
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    },

    toggleTodoItemComplete(todoItem: Todo, index: number) {
      this.todoItems.splice(index, 1, {
        ...todoItem,
        done: !todoItem.done,
      });
    },
  },

  created() {
    this.fetchTodoItems();
  },
});
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
}
</style>
