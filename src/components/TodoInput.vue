<template>
  <div>
    <label for="todo-input">오늘 할 일 : </label>
    <input
      type="text"
      id="todo-input"
      :value="item"
      @input="handleInput"
      @keyup.enter="addTodo"
    />&nbsp;
    <button @click="addTodo" type="button">추가</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  methods: {
    addTodo() {
      this.$emit('add');
    },
    handleInput(event: InputEvent) {
      const eventTarget = event.target as HTMLInputElement;
      this.$emit('input', eventTarget.value);
    },
  },
  props: {
    item: {
      type: String,
      required: true,
    },
  },
});
</script>

<style scoped></style>
