<template>
  <header>
    <h1>{{ title }}</h1>
    <Button text="Add Task" color="green"/>
  </header>
</template>

<script lang="ts">

import Button from './Button.vue'

export default{
  name: 'Header',
  props: {
    title: {
      type: String,
      default: 'Vue.js',
    }
  },
  components: {
    Button,
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;

}
</style>