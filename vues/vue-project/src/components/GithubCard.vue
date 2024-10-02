
<template>
  <div id="app" @click="redirectToGithub">
    <div>
      <img :src="userData.details.avatar_url" :alt="userData.details.login" style="width: 100px; height: 100px; border-radius: 50%;">
    </div>
    <ul>
      <li><strong>Login:</strong> {{ userData.details.login }}</li>
      <li v-if="userData.details.name"><strong>Name:</strong> {{ userData.details.name }}</li>
      <li v-if="userData.details.bio"><strong>Bio:</strong> {{ userData.details.bio }}</li>
      <li v-if="userData.details.location"><strong>Location:</strong> {{ userData.details.location }}</li>
      <li><strong>Public Repos:</strong> {{ userData.details.public_repos }}</li>
      <li><strong>Followers:</strong> {{ userData.details.followers }}</li>
      <li><strong>Following:</strong> {{ userData.details.following }}</li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  userData: Object,
});

const redirectToGithub = () => {
  window.open(props.userData.details.html_url, '_blank');
};
</script>

<style>
ul
{
  justify-content: center;
  height: 300px;
  width: 500px;
  padding: 1.5rem;
  margin-top: 25px;
  background: white;
  color: #48abe0;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 0 50px 15px #48abe0;
}
</style>


