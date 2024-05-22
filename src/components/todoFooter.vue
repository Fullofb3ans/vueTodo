<template>
  <footer class="footer">
    <span class="todo-count">{{ left }} Осталось выполнить</span>
    <ul class="filters">
      <li>
        <a @click="goTab('all')">All</a>
      </li>
      <li>
        <a @click="goTab('active')">Active</a>
      </li>
      <li>
        <a @click="goTab('completed')">Completed</a>
      </li>
    </ul>
    <button @click="clearCompleted" class="clear-completed">Clear completed</button>
  </footer>
</template>

<script setup>
defineProps(["left"]);
const emits = defineEmits([]);

function goTab(tab) {
  emits("goTab", tab);
}

function clearCompleted() {
  emits("clearCompleted");
}
</script>
