<script setup>
defineProps(["task", "index"]); // defineProps take input, parent pass data to child
defineEmits(["delete", "toggle"]); // defineEmits take call back function, child calls parent function
</script>

<template>
  <li
    class="group flex items-center justify-between px-2 py-1 rounded-md hover:bg-neutral-800 transition"
  >
    <!-- Left: task -->
    <span
      @click="$emit('toggle', task)"
      :class="[
        `mr-2 cursor-pointer`,
        task.completed ? ' text-neutral-500' : 'text-neutral-200',
      ]"
    >
      {{ task.completed ? "[X]" : "[ ]" }}
    </span>
    <span
      @click="$emit('toggle', task)"
      :class="[
        'cursor-pointer flex-1 text-md',
        task.completed ? 'line-through text-neutral-500' : 'text-neutral-200',
      ]"
    >
      {{ task.text }}
    </span>

    <div
      class="flex items-center gap-2 opacity-0 group-hover:opacity-100 transition"
    >
      <button
        @click="$emit('delete', index)"
        class="text-neutral-500 hover:text-red-400 cursor-pointer"
      >
        ✕
      </button>
    </div>
  </li>
</template>
