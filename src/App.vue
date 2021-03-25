<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Github cat Logo"
          contain
          :src="githubLogo"
          transition="scale-transition"
          width="40"
        />
      </div>
      <div class="d-flex align-center">
        <h1>CATS GALORE</h1>
      </div>
    </v-app-bar>
    <v-content>
      <Dashboard />
    </v-content>
  </v-app>
</template>

<script>
import Dashboard from './components/Dashboard';

export default {
  name: 'App',

  components: {
    Dashboard
  },

  data: () => ({
    githubLogo: require('@/assets/images/github-cat.svg')
  })
};
</script>
<style scoped>
h1 {
  margin-left: 1rem;
}
</style>
