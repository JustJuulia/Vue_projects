
<template>
  <div>
    <GithubCard v-for="(user, index) in usersData" :key="index" :userData="user" />
  </div>
</template>

<script setup>
import { ref, defineProps } from 'vue';
import GithubCard from './GithubCard.vue';

const props = defineProps({
  usersData: Array,
});
</script>

