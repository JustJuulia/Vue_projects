<template>
  <div class="github-card" @click="redirectToGithub">
    <div v-if="data">
      <img :src="data.avatar_url" alt="Avatar" />
      <div>
        <h2>{{ data.name }}</h2>
        <br />
        <p>{{ data.bio }}</p>
        <br />
        <p>Followers: {{ data.followers }}</p>
        <br />
        <p>Location: {{ data.location }}</p>
        <br />
      </div>
    </div>
    <div v-else>
      {{ errorMessage }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    username: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      data: null,
      errorMessage: ''
    }
  },
  methods: {
    async load() {
      try {
        const response = await fetch(`https://api.github.com/users/${this.username}`)
        if (!response.ok) {
          throw new Error(`User ${this.username} not found`)
        }
        const result = await response.json()
        this.data = result
      } catch (error) {
        console.error('Error loading user data:', error.message)
        this.errorMessage = `User ${this.username} not found`
      }
    },
    redirectToGithub() {
      if (this.data && this.data.html_url) {
        window.open(this.data.html_url, '_blank')
      }
    }
  },
  mounted() {
    this.load()
  }
}
</script>

<style scoped>
.github-card {
  box-shadow: 0 0 50px 15px #48abe0;
  display: flex;
  margin: 10px;
  justify-content: center;
  align-items: center;
  width: 450px;
  padding: 1.5rem;
  margin-top: 25px;
  background: white;
  color: #48abe0;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
}

.github-card img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin-right: 16px;
}

.github-card h2 {
  margin-top: 0;
}
</style>
