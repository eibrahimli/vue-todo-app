<template>
  <div>
    <input type="text" v-model="newTodo" class="todo-input" placeholder="Ediləcəklər...">
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: ''
    }
  }
}
</script>

<style lang='scss'>
  .todo-input{    
    width: 100%;
    padding: 16px 18px;
    font-size: 18px;
    margin-bottom: 16px;

    &:focus{
      outline: none;
    }
  }
</style>