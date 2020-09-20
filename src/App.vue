<template>
  <div id="app"></div>
</template>

<script>
import { randomNumBetween } from "./helperFunctions.js";

export default {
  name: "App",
  data: () => ({
    users: null,
    players: null,
  }),
  components: {},
  created() {
    this.setUp();
  },
  methods: {
    randomNumBetween,
    async setUp() {
      await this.getUsers();
      this.players = [this.pickPlayer(), this.pickPlayer()];
      console.log(this.players);
    },
    getUsers() {
      return fetch("https://jsonplaceholder.typicode.com/users")
        .then((response) => response.json())
        .then((json) => (this.users = json));
    },
    pickPlayer() {
      const randomIndex = this.randomNumBetween(0, this.users.length - 1);
      return {
        name: this.users[randomIndex].name,
        HP: this.randomNumBetween(-200, -40),
        DPS: this.randomNumBetween(5, 25),
      };
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
