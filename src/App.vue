<template>
  <div id="app"></div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    users: null,
  }),
  components: {},
  created() {
    this.setUp();
  },
  methods: {
    async setUp() {
      await this.getUsers();
    },
    getUsers() {
      return fetch("https://jsonplaceholder.typicode.com/users")
        .then((response) => response.json())
        .then((json) => (this.users = json));
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
