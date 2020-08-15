<template>
  <div style="width: 100%">
    <input type="text" v-model="newTodo" class="todo-input" placeholder="Ediləcəklər..." @keyup.enter="addTodo">
    <transition-group :duration='300' enter-active-class="animated fadeInUp" leave-active-class="fadeOutDown" name='fade'>
      <div v-for="(todo,index) in filteredTodos" :index="index" :key="todo.id" :title="todo.title" class="todo-item">
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" :class="{completed : todo.completed}" class="todo-item-label" @dblclick="editTodo(todo)">{{ todo.title }}</div>
          <input v-focus v-else class="todo-item-edit" @blur="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" @keyup.enter="doneEdit(todo)" type="text" v-model="todo.title">
        </div>      
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
      </div>
    </transition-group>
    <div class="extra-container">
      <div><label><input type="checkbox" :checked="anyRemaining" @click="checkAll()">Hamsını Seç</label></div>
      <div>{{ remaining }} maddə(lər)</div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{active: filter == 'all'}" @click="filter='all'">Bütün</button>
        <button :class="{active: filter == 'active'}" @click="filter='active'">Aktiv</button>
        <button :class="{active: filter == 'completed'}" @click="filter='completed'">Başa Çatan</button>
      </div>

      <div>
        <transition name="fade">
          <button v-if="clearCompletedButton" @click="clearCompletedTodos">Bitənləri Sil</button>  
        </transition>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  directives: {
    focus: {
      // directive definition
      inserted: function (el) {
        el.focus()
      }
    }
  },
  data() {
    return {
      newTodo: '',  
      cacheTitle: '',  
      filter: 'all' ,
      todos: [
        {
          'id': '1',
          'title': 'Vue öyrənməyə davam et',
          'completed' : false,
          'editing' : false,
        },
        {
          'id' : 2,
          'title' : 'React - a başla',
          'completed': false,
          'editing' : false,
        }
      ],
      id: 3,
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining == 0
    },
    clearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    },
    filteredTodos() {
      if(this.filter == 'all') {
        return this.todos
      } else if(this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if(this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    }
  },
  methods: {
    addTodo() {      
      if(this.newTodo.trim().length == 0) {
        return false  
      }
      this.todos.push({
        'id' : this.id++,
        'title' : this.newTodo,
        'completed' : false 
      })
      this.newTodo = ''
    },
    removeTodo(index) {
      this.todos.splice(index,1)
    },
    editTodo(todo)   {
      this.cacheTitle = todo.title
      todo.editing = true    
    },
    doneEdit(todo) {
      if(todo.title.trim() == '') {
        todo.title = this.cacheTitle
      }
      todo.editing = false
    },
    cancelEdit(todo)  {
      todo.title = this.cacheTitle
    },

    // Hamısını Seç
    checkAll() {      
      this.todos.forEach(todo => todo.completed = event.target.checked)
    },

    //Başa çatmış maddələri sil
    clearCompletedTodos() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style lang='scss'>

  @import url("https://cdn.jsdelivr.net/npm/animate.css@3.5.1");
  
  .todo-input{      
    width: 100%;
    padding: 16px 18px;
    font-size: 18px;
    margin-bottom: 16px;

    &:focus{
      outline: none;
    }
  }
  .todo-item{
    width: 100%;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .remove-item {
    cursor: pointer;
    margin: 0 0 0 14px;
    &:hover{
      color: black;
    }
  }
  .todo-item-left{
    display: flex;
    align-items: center;
  }
  .todo-item-label{
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit{
    font-size: 24px;
    color: #2c3e50;
    width: 100%;
    margin-left: 12px;
    border: 1px solid #ccc;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    padding: 10px;

    &:focus{
      outline: none;
    }
  }
  .completed{
    text-decoration: line-through;
    color: gray;
  }  

  .extra-container{
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 14px;
    font-size: 16px;
    border-top: 1px solid lightgray;
    margin-bottom: 14px;
  }
  button{
    font-size: 14px;
    background-color: white;
    appearance: none;

    &:hover{
      background: lightgreen;
    }
    &:focus{
      outline: none
    }
  }
  .active{
    background: lightgreen
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
</style>