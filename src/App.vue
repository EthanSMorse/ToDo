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

  function activeFilter (todo) {
    return todo.complete == false
  }

  function completeAll () {
    for (let todo of todos.value) {
      todo.complete = true
    }

  }
  
  function deleteAll () {
    todos.value = []
  }
</script>

<template>
  <h1 class="h1-box">My ToDo Application</h1>

  <div>
    <button id="selectAll-style" @click="completeAll">Complete All</button>
    <input id="input-style" @keydown.enter ="addToDo" v-model="input">
    <button id="AddTD-style" @click="addToDo">Add ToDo</button>
    <br>
    <button id="deleteAll-style" @click="deleteAll">Delete All</button>
  </div>

  <br>
  <br>

  <main>
    <p id="counter-style" v-if="todos.length > 0">{{ todos.filter(activeFilter).length }} items left</p>

    <div v-if="todos.length > 0">
      <form>
          <input type="radio" id="all" value="all" name="filter" v-model="filter">
          <label for="all">All</label>
          <input type="radio" id="active" value="active" name="filter" v-model="filter">
          <label for="active">Active</label>
          <input type="radio" id="complete" value="completed" name="filter" v-model="filter">
          <label for="complete">Complete</label>
      </form>
    </div>

    <div class="li-style li-box" v-for="(todo, index) in todos.filter(todoFilter)" :class="{completed:todo.complete}">
      <button @click= "deleteToDo(index)">Delete</button>
      <label class="container">
        <input type="checkbox" v-model="todo.complete">
        <span class="checkmark"></span>
      </label>
      {{ todo.text }}
    </div>
  </main>
</template>

<style>

  body {
    background-color: forestgreen;
    text-align: center;
    font-family: cursive;
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

  #deleteAll-style {
    margin-right: 660px;
    margin-top: 20px;
  }

  #AddTD-style {
    margin-left: 20px;
  }

  .li-style {
    list-style: none;
    color: gold;
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
    color: goldenrod;
  }

  #counter-style {
    font-size: 30px;
    color: gold;
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
  background-color: chartreuse;
}

.container:hover input ~ .checkmark {
  background-color: forestgreen;
}

.container input:checked ~ .checkmark {
  background-color: chartreuse;
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
  border: solid gold;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

[type="radio"]:checked,
[type="radio"]:not(:checked) {
    position: absolute;
    left: -9999px;
}
[type="radio"]:checked + label,
[type="radio"]:not(:checked) + label
{
    position: relative;
    padding-left: 40px;
    cursor: pointer;
    line-height: 20px;
    display: inline-block;
    color: gold;
    font-size: 25px;
    margin: 10px;
}
[type="radio"]:checked + label:before,
[type="radio"]:not(:checked) + label:before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    width: 25px;
    height: 25px;
    border: 1px solid goldenrod;
    border-radius: 100%;
    background: chartreuse;
}
[type="radio"]:checked + label:after,
[type="radio"]:not(:checked) + label:after {
    content: '';
    width: 12px;
    height: 12px;
    background: gold;
    position: absolute;
    top: 7.6px;
    left: 7.6px;
    border-radius: 100%;
    -webkit-transition: all 0.2s ease;
    transition: all 0.2s ease;
}
[type="radio"]:not(:checked) + label:after {
    opacity: 0;
    -webkit-transform: scale(0);
    transform: scale(0);
}
[type="radio"]:checked + label:after {
    opacity: 1;
    -webkit-transform: scale(1);
    transform: scale(1);
}
</style>