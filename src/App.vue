<script setup>
  import {ref, watch} from 'vue'
  let input = ref ("")
  let todos = ref (JSON.parse(window.localStorage.getItem('todos')) ?? [])

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
  <h1 class="h1-box">My ToDo Application</h1>

  <button id="selectAll-style">Select All</button>
  <input id="input-style" @keydown.enter ="addToDo" v-model="input">
  <button id="AddTD-style" @click="addToDo">Add ToDo</button>

    <div class="li-style li-box" v-for="(todo, index) in todos" :class="{completed:todo.complete}">
      <button @click= "deleteToDo(index)">Delete</button>
      <input class="CB-style" type="checkbox" v-model="todo.complete">
      {{ todo.text }}
    </div>
</template>

<style>

  body {
    background-color: forestgreen;
    text-align: center;
  }

  h1 {
    color: gold;
  }

  .h1-box {
    background-color: rgb(45, 182, 45);
    height: fit-content;
    width:fit-content;
    padding: 10px;
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
    border-color: goldenrod;
    margin-top: 17.5px;
    margin-left: 575px;
    margin-right: 300px;
  }

  #input-style {
    height: 50px;
    width: 500px;
    padding: 10px;
    border-style: solid;
    border: 10px;
    border-radius: 12px;
    font-size: 30px;
  }

  button {
    background-color: chartreuse;
    height: 50px;
    width: 100px;
    border-style: solid;
    border-color: gold;
    border-width: 2px;
    border-radius: 10px;
  }

  #selectAll-style {
    margin-right: 20px;
  }

  #AddTD-style {
    margin-left: 20px;
  }

  .li-style {
    list-style: none;
    font-size: 50px;
    color: gold;
    margin: 10px;
    font-size: 25px;
  }

  .li-box {
    background-color: rgb(45, 182, 45);
    height: fit-content;
    padding: 10px;
    min-width: 11px;
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
    border-color: goldenrod;
    margin-top: 17.5px;
    margin-left: 300px;
    margin-right: 300px;
    text-align: left;
  }

  .completed {
    text-decoration: line-through;
    color: gray;
  }
</style>