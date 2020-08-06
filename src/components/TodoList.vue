<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done">
  </div>
</template>

<script>
export default {

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