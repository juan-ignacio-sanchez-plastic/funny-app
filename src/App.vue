<template>
  <div id="app">
    <Player v-if="players.length === 2" :user="players[0]" :pickAndSetPlayer="pickAndSetPlayer1" />
    <div class="middle-container">
      <WinnerMessage v-if="winner" :winner="winner"></WinnerMessage>
      <button v-if="!arePlaying" @click="startCompetition" class="btn-compete">Compete!</button>
    </div>
    <Player v-if="players.length === 2" :user="players[1]" :pickAndSetPlayer="pickAndSetPlayer2" />
  </div>
</template>

<script>
import Player from "./components/Player";
import WinnerMessage from "./components/WinnerMessage";
import { randomNumBetween } from "./helperFunctions.js";
import Vue from "vue";
import VueConfetti from "vue-confetti";

Vue.use(VueConfetti);

export default {
  name: "App",
  data: () => ({
    users: [],
    players: [],
    arePlaying: false,
    winner: null,
    audios: {
      laugh: require("./assets/audios/Laugh.mp3"),
    },
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
      this.pickAndSetPlayer1();
      this.pickAndSetPlayer2();
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
    async pickAndSetPlayer1() {
      const player1 = await this.pickPlayer();
      this.setPlayer1(player1);
    },
    async pickAndSetPlayer2() {
      const player2 = await this.pickPlayer();
      this.setPlayer2(player2);
    },
    async pickPlayer() {
      const image = await this.getImage();
      const randomIndex = this.randomNumBetween(0, this.users.length - 1);
      return {
        name: this.users[randomIndex].name,
        HP: this.randomNumBetween(-200, -40),
        DPS: this.randomNumBetween(5, 25),
        image,
      };
    },
    setPlayer1(player1) {
      const newPlayers = [...this.players];
      newPlayers[0] = player1;
      this.players = newPlayers;
    },
    setPlayer2(player2) {
      const newPlayers = [...this.players];
      newPlayers[1] = player2;
      this.players = newPlayers;
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
      this.playLaugh();
      if (this.players[0].HP > 0 && this.players[1].HP > 0) {
        this.winner = "TIE";
      } else if (this.players[0].HP > 0) {
        this.winner = this.players[1].name;
        this.startConfetti();
      } else {
        this.winner = this.players[0].name;
        this.startConfetti();
      }
    },
    startConfetti() {
      this.$confetti.start({
        particlesPerFrame: 0.75,
      });
    },
    playLaugh() {
      const laughAudio = new Audio(this.audios.laugh);
      laughAudio.play();
    },
  },
};
</script>

<style lang="scss">
@import "~normalize.css";
@import "./scss/globalStyles.scss";
#app {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 100vh;
  background-image: url("./assets/bg1.jpg");
  background-size: cover;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: $font-color;
}
.btn-compete {
  font-size: 2rem;
  -webkit-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  -moz-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
}
.middle-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
