<script setup>
import { ref } from "vue";
const props = defineProps(["todo", "page"]);

const newText = ref(props.todo.name);

const emits = defineEmits([]);

function changeCompleted() {
  emits("change-completed", props.todo.id);
}
function deleteIt() {
  emits("deleteIt", props.todo.id);
}
function changeText() {
  emits("changeText", newText);
  isEditing.value = false;
}

const isEditing = ref(false);
</script>

<template>
  <li
    class="todo"
    :class="{
      completed: todo.isChecked,
      editing: isEditing,
      hidden: props.todo.show,
    }"
  >
    <div class="view" @dblclick="isEditing = true">
      <input
        class="toggle"
        type="checkbox"
        @input="changeCompleted"
        :checked="todo.isChecked"
      />
      <label>{{ todo.name }} #{{ todo.id }}</label>
      <button class="destroy" @click="deleteIt"></button>
    </div>
    <input class="edit" type="text" @keyup.enter="changeText" v-model="newText" />
  </li>
</template>
