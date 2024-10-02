<template>
    <div>
      <input type="text" v-model="newTask" @keyup.enter="createTask">
      <button @click="createTask">Dodaj</button>
    </div>
  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  
  const newTask = ref('');
  const emit = defineEmits(['create']);
  
  const createTask = () => {
    if (newTask.value.trim() !== '') {
      emit('create', newTask.value);
      newTask.value = '';
    }
  };
  </script>
  