<template>
  <section class="todoapp">
    <todoHeader @newTodo="addTodo"></todoHeader>
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox" @click="toogleAll" />
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <todoFeatures
          v-for="todo in todos"
          @change-completed="changeCompleted"
          @deleteIt="deleteIt"
          @changeText="changeText($event, todo.id)"
          :key="todo.id"
          :todo="todo"
          :page="page"
        ></todoFeatures>
      </ul>
    </section>
  </section>
  <todoFooter @clearCompleted="clearCompleted" @goTab="goTab" :left="left"></todoFooter>
</template>

<script setup>
import todoFeatures from "@/components/todoFeatures.vue";
import todoHeader from "@/components/todoHeaderInput.vue";
import todoFooter from "@/components/todoFooter.vue";
import { ref } from "vue";
import { reactive } from "vue";
import { computed } from "vue";

const page = ref("all");

const todos = reactive([
  {
    id: 1,
    name: "Сделать уборку",
    isChecked: true,
  },
  {
    id: 2,
    name: "Сделать дз",
    isChecked: false,
  },
  {
    id: 3,
    name: "Готовка",
    isChecked: true,
  },
  {
    id: 4,
    name: "Сушка",
    isChecked: false,
  },
]);

function addTodo(newTitle) {
  todos.push({
    id: todos.length + 1,
    name: newTitle,
    isChecked: false,
  });
}

function changeCompleted(todoId) {
  let el = todos.find((todo) => todo.id == todoId);
  console.log(el);
  console.log(todoId);
  el.isChecked = !el.isChecked;
}

function deleteIt(todoId) {
  let index = todos.findIndex((todo) => todo.id == todoId);
  todos.splice(index, 1);
}

function changeText(txt, todoId) {
  let el = todos.find((todo) => todo.id == todoId);
  el.name = txt;
}

function clearCompleted() {
  let index = todos.findIndex((todo) => todo.isChecked == true);
  while (index !== -1) {
    todos.splice(index, 1);
    index = todos.findIndex((todo) => todo.isChecked == true);
  }
}

function goTab(tab) {
  if (tab == "active") {
    todos.forEach((todo) => {
      todo.show = "";

      if (todo.isChecked == true) {
        todo.show = true;
      }
    });
  } else if (tab == "completed") {
    todos.forEach((todo) => {
      todo.show = "";
      if (todo.isChecked == false) {
        todo.show = true;
      }
    });
  } else if (tab == "all") {
    todos.forEach((todo) => {
      todo.show = false;
    });
  }
}

function toogleAll() {
  let count = 0;
  todos.map((item) => {
    if (item.isChecked == false) {
      return count++;
    }
  });
  todos.map((item) => {
    console.log(count);
    if (count == 0) {
      item.isChecked = false;
    } else {
      todos.map((item) => {
        item.isChecked = true;
      });
    }
  });
}

const left = computed(() => todos.filter((todo) => todo.isChecked == false).length);
</script>
