<template>
  <div class="github-card-wrapper">
    <github-card v-for="(user, index) in users" :key="index" :username="user" />
  </div>
</template>

<script>
import GithubCard from './GithubCard.vue'

export default {
  props: {
    users: {
      type: Array,
      required: true
    }
  },
  components: {
    GithubCard
  }
}
</script>

<style scoped>
.github-card-wrapper {
  display: flex;
}
</style>
