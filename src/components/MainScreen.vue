<template>
  <h1>ToDo Application</h1>
  <div class="screen__form">
    <p>What do you plan on doing?</p>
    <form @submit.prevent="onSubmit">
      <input type="text" v-model="text" placeholder="Type here" />
      <button>Add todo</button>
    </form>
  </div>
  <div class="screen__list">
    <h3 v-if="listTodo.length === 0">No ToDo here</h3>
    <ul v-else-if="listTodo.length">
      <div class="list__control" v-for="todo in listTodo">
        <div class="complete__list">
          <input type="checkbox" v-model="todo.completed" />
          <p :class="{ completed: todo.completed }">{{ todo.plan }}</p>
        </div>
        <div class="list__btn">
          <button @click="editList(todo)" class="deleteBtn">Edit</button>
          <button @click="deleteList(todo)" class="deleteBtn">Delete</button>
        </div>
      </div>
    </ul>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";
const text = ref("");
const isEdit = ref(false);
const idEdit = ref(null);
const listTodo = reactive([]);
let id = 0;
const onSubmit = () => {
  if (isEdit.value) {
    console.log("=>> listTodo", listTodo);
    console.log("=>> idEdit", idEdit);

    let index = listTodo.findIndex((todo) => todo.id === idEdit.value);
    if (index !== -1) {
      listTodo[index].plan = text.value;
      isEdit.value = false;
      idEdit.value = null;
      text.value = "";
    }
  } else {
    listTodo.push({ id: id++, plan: text.value, completed: false });
    text.value = "";
  }
};
const deleteList = (todo) => {
  listTodo.splice(listTodo.indexOf(todo), 1);
};
const editList = (todo) => {
  idEdit.value = todo.id;
  isEdit.value = true;
  text.value = todo.plan;
};
</script>

<style scoped>
.screen__form {
  border: 0.5px solid #2f2f2f;
  border-radius: 5px;
  padding: 1rem;
  margin: 20px 0;
  form {
    width: 100%;
  }

  input {
    width: 80%;
    height: 40px;
    background-color: #fff !important;
    padding: 10px;
    border-radius: 5px;
    margin-top: 5px;
    margin-bottom: 20px;
  }

  button {
    width: 80%;
    background-color: #008000;
    color: #fff;
    font-size: 18px;
    &:hover {
      opacity: 0.8;
      cursor: pointer;
    }
  }
}
.screen__list {
  border: 0.5px solid #2f2f2f;
  border-radius: 5px;
  padding: 1rem;
  color: #fff;
  ul {
    list-style-type: none;
    .list__control {
      background-color: #17181d;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 5px;
      padding: 10px 15px;
      .complete__list {
        display: flex;
        p {
          margin-left: 5px;
          &.completed {
            color: #828283;
            text-decoration: line-through;
          }
        }
      }
      .list__btn {
        .deleteBtn {
          margin-left: 5px;
          width: fit-content;
          height: fit-content;
          padding: 4px 6px;
          background-color: #ff5b57;
          color: #fff;
          &:hover {
            opacity: 0.8;
            cursor: pointer;
          }
          &:focus {
            outline: none;
          }
        }
      }
    }
  }
}
</style>
