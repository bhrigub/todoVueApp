<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from './components/Todos';
import Header from './components/layout/Header';
import AddTodo from './components/AddTodo';
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return{
      todos: [
      //   {
      //     id: 1,
      //     title: "No 1",
      //     completed: false

      // }, 
      // {
      //     id: 2,
      //     title: "No 2",
      //     completed: true

      // },
      // {
      //     id: 3,
      //     title: "No 3",
      //     completed: false

      // }
      ]
    }
  },
  methods: {
    deleteTodo(id){
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => this.todos = this.todos.filter(todo => todo.id!==id,res.data))
      .catch(err => console.log(err));      
    },
    addTodo(newTodo){
      const {title,completed}=newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
      .then(res=>this.todos= [...this.todos, res.data])
      .catch(err=> console.log(err));
      
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=6')
    .then(res=> this.todos=res.data)
    .catch(err=> console.log(err));

  }
}
</script>

<style>
  *{
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  
  body {
    /*font-family: 'Pacifico', cursive;*/
    font-family: 'Rancho', cursive, 'Courier New', monospace;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
