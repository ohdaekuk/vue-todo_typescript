<template>
  <div>
    <label for="todo-input">오늘 할 일 : </label>
    <input
      type="text"
      id="todo-input"
      :value="item"
      @input="handleInput"
      @keyup.enter.prevent="addTodo"
    />&nbsp;
    <button @click="addTodo" type="button">추가</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { VueEvent } from '@/types/index';

export default Vue.extend({
  methods: {
    addTodo() {
      this.$emit('add');
    },
    handleInput(event: VueEvent.Input<HTMLInputElement>) {
      // console.log(event.target.value);

      // const eventTarget = event.target as HTMLInputElement;
      const value = event.target.value;
      this.$emit('input', value);
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
