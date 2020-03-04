<template>
  <div class="cell" v-on:click="$emit('performMove')">
    <span v-if="value !== ''">{{ value }}</span>
  </div>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    value: String,
    i: Number,
    j: Number
  }
};
</script>

<style>
.cell {
  width: 64px;
  height: 64px;
  border: 2px solid rgb(75, 75, 75);
  font-size: 42px;
  cursor: pointer;
}
</style>
