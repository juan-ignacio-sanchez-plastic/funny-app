<template>
  <div id="app">
    <Player v-if="players" :user="players[0]" />
    <div class="middle-container">
      <WinnerMessage v-if="winner" :winner="winner"></WinnerMessage>
      <button v-if="!arePlaying" @click="startCompetition" class="btn-compete">Compete!</button>
    </div>
    <Player v-if="players" :user="players[1]" />
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
    users: null,
    players: null,
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
@import "./scss/_variables.scss";
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
  padding: 15px;
  border: 0px;
  border-radius: 50px;
  -webkit-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  -moz-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  transition: transform 0.2s;
  &:hover {
    transform: scale(1.2);
    cursor: pointer;
  }
}
.middle-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
