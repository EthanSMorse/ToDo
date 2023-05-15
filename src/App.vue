<script setup>
  import {ref, watch} from 'vue'
  let input = ref ("")
  let todos = ref (JSON.parse(window.localStorage.getItem('todos')))

  watch(todos, function(value) {
    window.localStorage.setItem('todos', JSON.stringify(value))
  }, {deep: true})    

  function addToDo () {
    if (input.value == "") {
      alert ("Please put something in the text box.")
    }
    else {
      todos.value.push({text: input.value, complete: false})
    }
    input.value = ""
  }

  function deleteToDo (index) {
    todos.value.splice(index, 1)
  }
</script>

<template>
  <h1>My ToDo Application</h1>

  <input @keydown.enter ="addToDo" v-model="input">
  <button id="AddTD-style" @click="addToDo">Add ToDo</button>

  <div class="li-box">
    <div class="li-style" v-for="(todo, index) in todos">
      <button @click= "deleteToDo(index)">Delete</button>
      <input class="CB-style" type="checkbox" v-model="todo.complete">
      {{ todo.text }}
    </div>
  </div>
</template>

<style>
  button {
    background-color: chartreuse;
    height: 50px;
    width: 100px;
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
  }

  .CB-style {
    
  }

  #AddTD-style {
    margin-left: 20px;
    border-color: gold;
  }

  .li-style {
    list-style: none;
    font-size: larger;
    margin: 10px;
  }

  .li-box {
    background-color: forestgreen;
    height: fit-content;
    padding: 10px;
    min-width: 11px;
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
    border-color: gold;
    margin-top: 35px;
  }
</style>