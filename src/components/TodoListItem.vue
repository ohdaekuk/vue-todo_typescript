<template>
  <ul>
    <li>
      <span class="item" :class="todoItemClass" @click="toggleItem(index)">{{
        todoItem.title
      }}</span>
      &nbsp;
      <button @click="removeTodo(index)">삭제</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { Todo } from '@/App.vue';
import Vue from 'vue';
import { PropType } from 'vue/types/v3-component-props';

export default Vue.extend({
  props: {
    todoItem: {
      type: Object as PropType<Todo>,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  computed: {
    todoItemClass(): string | null {
      return this.todoItem.done ? 'complete' : null;
    },
  },
  methods: {
    removeTodo(index: number) {
      //   console.log(index);

      this.$emit('remove', index);
    },
    toggleItem(index: number) {
      this.$emit('toggle', this.todoItem, index);
    },
  },
});
</script>

<style scoped>
.item {
  cursor: pointer;
}
.complete {
  text-decoration: line-through;
}
</style>
