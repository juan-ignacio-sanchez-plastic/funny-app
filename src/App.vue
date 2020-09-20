<template>
  <div id="app">
    <Player v-if="players" :user="players[0]" />
    <button v-if="!arePlaying" @click="startCompetition" class="btn-compete">Compete!</button>
    <WinnerMessage v-if="winner" :winner="winner"></WinnerMessage>
    <Player v-if="players" :user="players[1]" />
  </div>
</template>

<script>
import Player from "./components/Player";
import WinnerMessage from "./components/WinnerMessage";
import { randomNumBetween } from "./helperFunctions.js";

export default {
  name: "App",
  data: () => ({
    users: null,
    players: null,
    arePlaying: false,
    winner: null,
  }),
  components: {
    Player,
    WinnerMessage,
  },
  created() {
    this.setUp();
  },
  methods: {
    randomNumBetween,
    async setUp() {
      await this.getUsers();
      const imageP1 = await this.getImage();
      const imageP2 = await this.getImage();
      this.players = [
        {
          ...this.pickPlayer(),
          image: imageP1,
        },
        {
          ...this.pickPlayer(),
          image: imageP2,
        },
      ];
    },
    getUsers() {
      return fetch("https://jsonplaceholder.typicode.com/users")
        .then((response) => response.json())
        .then((json) => (this.users = json));
    },
    getImage() {
      return fetch("https://api.thecatapi.com/v1/images/search")
        .then((response) => response.json())
        .then((json) => json[0].url);
    },
    pickPlayer() {
      const randomIndex = this.randomNumBetween(0, this.users.length - 1);
      return {
        name: this.users[randomIndex].name,
        HP: this.randomNumBetween(-200, -40),
        DPS: this.randomNumBetween(5, 25),
      };
    },
    startCompetition() {
      this.arePlaying = true;
      const playing = setInterval(() => {
        this.players[0].HP += this.players[1].DPS;
        this.players[1].HP += this.players[0].DPS;
        if (this.players[0].HP > 0 || this.players[1].HP > 0) {
          this.arePlaying = false;
          clearInterval(playing);
          this.setWinner();
        }
      }, 1000);
    },
    setWinner() {
      if (this.players[0].HP > 0 && this.players[1].HP > 0) {
        this.winner = "TIE";
      } else if (this.players[0].HP > 0) {
        this.winner = this.players[1].name;
      } else {
        this.winner = this.players[0].name;
      }
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
