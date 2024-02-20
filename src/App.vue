<script setup>
import { ref } from "vue";

// Initialize todo item and todolist as null/empty
let todoList = ref([]);
let todo = ref(null);

// Adding todo to the list
const addTodo = () => {
  if (validateTodo(todo.value)) {
    let id = todoList.value.length;
    todoList.value.push({
      id: ++id,
      todo: todo.value,
      is_done: false,
    });
    todo.value = null;
  }
};

// Validate existing todos
const validateTodo = (newTodo) => {
  let index = todoList.value.findIndex((t) => t.todo === newTodo);
  if (index !== -1) {
    alert("Todo Already Exist");
    return false;
  } else {
    return true;
  }
};

// called when a todo will be updated from the list
let selectedTodo = ref(null);
const selectTodo = (item) => {
  selectedTodo.value = item;
  todo.value = item.todo;
};

// method to update/edit the todo item
const updateTodo = () => {
  if (validateTodo(todo.value)) {
    let index = todoList.value.findIndex((t) => t.id === selectedTodo.value.id);
    index !== -1 && (todoList.value[index].todo = todo.value);
  }

  todo.value = selectedTodo.value = null;
};

// Mark todo as done
const markAsDone = (item) => {
  if (confirm(`Are you sure you want to mark ${item.todo} as done?`)) {
    let index = todoList.value.findIndex((t) => t.id === item.id);
    index !== -1 && (todoList.value[index].is_done = true);
  }
};

// Removing a todo item
const removeTodo = (item) => {
  if (confirm(`Are you sure you want to remove ${item.todo}?`)) {
    let index = todoList.value.findIndex((t) => t.id === item.id);
    index !== -1 && todoList.value.splice(index, 1);
  }
};
</script>

<template>
  <div class="d-flex justify-content-center container">
    <div class="w-75">
      <form @submit.prevent="!selectedTodo ? addTodo() : updateTodo()">
        <div class="mb-3 mt-5">
          <label for="todo">What's your todo?</label>
          <div class="d-flex">
            <input
              v-model="todo"
              type="text"
              class="form-control"
              id="todo"
              placeholder="eg. Create todo application"
            />
            <button v-if="!selectedTodo" class="btn btn-primary ms-3">
              <i class="bi bi-plus-circle-fill"></i>
            </button>
            <button class="btn btn-primary ms-3">
              <i class="bi bi-pencil-square"></i>
            </button>
          </div>
        </div>
      </form>

      <div
        @click="selectTodo(item)"
        v-for="item in todoList"
        :key="item.id"
        class="toast show mt-2 w-100"
      >
        <div class="toast-header">
          <strong class="me-auto">{{ item.todo }}</strong>
          <button
            :class="`btn btn-${
              item.is_done ? 'success' : 'outline-secondary'
            } btn-sm`"
            @click.stop="markAsDone(item)"
          >
            <i class="bi bi-check-lg"></i>
          </button>

          <button
            class="btn btn-danger btn-sm ms-1"
            @click.stop="removeTodo(item)"
          >
            <i class="bi bi-trash-fill"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
