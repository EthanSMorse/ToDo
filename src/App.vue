<script setup>
  import {ref} from 'vue'
  let input = ref ("")
  let todos = ref ([])

  function addToDo () {
    todos.value.push(input.value)
    input.value = ""
  }

  function deleteToDo (index) {
    todos.value.splice(index, 1)
  }
</script>

<template>
  <h1>My ToDo Application</h1>

  <input @keydown.enter ="addToDo" v-model="input">
  <br>
  <br>
  <button   @click="addToDo">Add ToDo</button>

  <ul>
    <li class="li-style" v-for="(todo, index) in todos">
      <button @click= "deleteToDo(index)">Delete</button>
      {{ todo }}
    </li>
  </ul>
</template>

<style>
  button {
    background-color: chartreuse;
    height: 50px;
    width: 100px;
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
    border-color: forestgreen;
  }

  .li-style {
    list-style: none;
  }
</style>