<script setup lang="ts">
  import { uid } from "uid";
  import { ref } from "vue";
  import TodoCreator from "../components/TodoCreator.vue";
  import TodoItem from "../components/TodoItem.vue";

  const todoList = ref([] as any);
  const createTodo = (todo: string) => {
      const data = {
        id: uid(),
        todo,
        isCompleted: false,
        isEditing: null,
      };
      todoList.value.push(data);
  };

  const toggleEditTodo = (todoPos: number) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
  };

  const updateTodo = (todoVal: any, todoPos: number) => {
    todoList.value[todoPos].todo = todoVal?.value;
  };

  const toggleTodoComplete = (todoPos: number) => {
    todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
  };

  const deleteTodo = (todoPos: number) => { 
      todoList.value = todoList.value.filter((item: any, index: number) => index != todoPos);
  };
  
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo"/>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @toggle-complete="toggleTodoComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
  
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>