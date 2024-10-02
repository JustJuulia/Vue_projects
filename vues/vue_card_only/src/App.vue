<template>
  <div id="app">
    <github-card-wrapper :users="usernames" />
  </div>
</template>

<script>
import GithubCardWrapper from './components/GithubCardWrapper.vue'

export default {
  components: {
    GithubCardWrapper
  },
  data() {
    return {
      usernames: [
        'MattMoszczynski',
        'lwadas',
        'JustJuulia',
        'totalnieprawidlowyusernamewcalebladniewyskoczyexe',
        'amystas'
      ]
    }
  }
}
</script>

