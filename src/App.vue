<script setup>
import { computed, onMounted, ref, watch } from "vue";
import TaskItem from "./components/taskItem.vue";

const input = ref("");
const tasks = ref([]);

onMounted(() => {
  const saved = localStorage.getItem("tasks");
  if (saved) {
    tasks.value = JSON.parse(saved);
  }
});

const addTask = () => {
  if (input.value.trim()) {
    tasks.value.push({
      text: input.value,
      completed: false,
    });
    input.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

const toggleTask = (task) => {
  task.completed = !task.completed;
};

const clearAllTasks = () => {
  tasks.value = [];
};
watch(
  tasks,
  (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
  { deep: true },
);

const totalTasks = computed(() => tasks.value.length);
const completedTasks = computed(
  () => tasks.value.filter((t) => t.completed).length,
);
</script>

<template>
  <div
    class="min-h-screen w-full bg-neutral-950 text-neutral-200 flex justify-center sm:px-10 px-5 py-10 font-mono"
  >
    <div class="w-full">
      <!-- Title -->
      <h1 class="text-4xl mb-6 text-neutral-200">Taskify</h1>

      <!-- Input (minimal) -->
      <input
        v-model="input"
        @keyup.enter="addTask"
        placeholder="type task and press enter..."
        class="bg-transparent w-full sm:w-xl text-xl sm:text-3xl size-20 pb-2 outline-none placeholder-neutral-600"
      />

      <!-- Stats -->
      <div
        v-if="tasks.length > 0"
        class="flex justify-between max-w-xl pe-2  text-neutral-500 mt-4"
      >
        <div class="flex sm:gap-5 gap-3 text-md">
          <span>total: <span class="text-white">{{ totalTasks }}</span></span>
          <span>completed: <span class="text-white">{{ completedTasks }}</span></span>
        </div>
        <button
          @click="clearAllTasks"
          class="text-red-800 hover:text-red-600 ml-4 sm:ml-20 cursor-pointer text-md text-start"
        >
          Delete All
        </button>
      </div>

      <!-- Empty -->
      <p v-if="tasks.length === 0" class="mt-4 text-neutral-600 text-md">
        No tasks yet...
      </p>

      <!-- List -->
      <ul class="mt-6 space-y-2 max-w-xl">
        <TaskItem
          v-for="(task, index) in tasks"
          :key="index"
          :task="task"
          :index="index"
          @delete="deleteTask"
          @toggle="toggleTask"
        />
      </ul>
    </div>
  </div>
</template>
