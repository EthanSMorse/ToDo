<script setup>
  import {ref, watch} from 'vue'
  let input = ref ("")
  let todos = ref (JSON.parse(window.localStorage.getItem('todos')) ?? [])
  let filter = ref ([])

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

  function todoFilter (todo) {
    if (filter.value == 'active') {
      return todo.complete == false
    } else if (filter.value == 'completed') {
      return todo.complete == true
    } else {
      return true 
    }
  }
</script>

<template>
  <h1 class="h1-box">My ToDo Application</h1>

  <div>
    <button id="selectAll-style">Select All</button>
    <input id="input-style" @keydown.enter ="addToDo" v-model="input">
    <button id="AddTD-style" @click="addToDo">Add ToDo</button>
  </div>

  <br>
  <br>

  <div>
    <input name="filter" type="radio" value="all" v-model="filter">
    <input name="filter" type="radio" value="active" v-model="filter">
    <input name="filter" type="radio" value="completed" v-model="filter">
  </div>

  <div class="li-style li-box" v-for="(todo, index) in todos.filter(todoFilter)" :class="{completed:todo.complete}">
    <button @click= "deleteToDo(index)">Delete</button>
    <label class="container">
      <input id="CB-style" type="checkbox" v-model="todo.complete">
      <span class="checkmark"></span>
    </label>
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
    font-family: cursive;
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
    margin-left: 595px;
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
    font-family: cursive;
  }

  button {
    background-color: chartreuse;
    height: 50px;
    width: 100px;
    border-style: solid;
    border-color: gold;
    border-width: 2px;
    border-radius: 10px;
    font-family: cursive;
  }

  #selectAll-style {
    margin-right: 20px;
  }

  #AddTD-style {
    margin-left: 20px;
  }

  .li-style {
    list-style: none;
    color: gold;
    margin: 10px;
    font-size: 25px;
    font-family: cursive;
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

  #CB-style {
  }

  .container {
  display: inline-block;
  position: relative;
  padding-left: 35px;
  margin-left: 15px;
  margin-right: 5px;
  margin-bottom: 17px;
  cursor: pointer;
  font-size: 22px;
}

.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
  margin-top: 10px;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

.container:hover input ~ .checkmark {
  background-color: #ccc;
}

.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.container input:checked ~ .checkmark:after {
  display: block;
}

.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>