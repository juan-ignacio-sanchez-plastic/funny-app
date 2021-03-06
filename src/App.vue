<template>
  <div id="app">
    <div class="player-container">
      <Player v-if="players.length === 2" :user="players[0]" :pickAndSetPlayer="pickAndSetPlayer1" />
    </div>
    <div class="middle-container">
      <WinnerMessage v-if="winner" :winner="winner"></WinnerMessage>
      <button v-if="!arePlaying && !winner" @click="startCompetition" class="btn-principal">Compete!</button>
      <button v-if="winner" @click="playAgain" class="btn-principal">Play again!</button>
    </div>
    <div class="player-container">
      <Player v-if="players.length === 2" :user="players[1]" :pickAndSetPlayer="pickAndSetPlayer2" />
    </div>
    <audio class="audioGameMusic">
      <source src="./assets/audios/GameMusic.mp4" type="audio/mpeg" />Your browser does not support the audio element.
    </audio>
    <audio class="audioLaugh">
      <source src="./assets/audios/Laugh.mp3" type="audio/mpeg" />Your browser does not support the audio element.
    </audio>
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
      laugh: null,
      gameMusic: null,
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
      this.audios.laugh = document.querySelector(".audioLaugh");
      this.audios.gameMusic = document.querySelector(".audioGameMusic");
      this.audios.gameMusic.volume = 1;
      this.audios.laugh.currentTime = 0;
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
        HP: this.randomNumBetween(-200, -100),
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
      this.audios.gameMusic.play();
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
      this.reduceVolumeForAudio("gameMusic");
      this.audios.laugh.play();
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
    reduceVolumeForAudio(audioName) {
      const audioReducer = setInterval(() => {
        this.audios[audioName].volume -= 0.05;
        if (this.audios[audioName].volume <= 0.2) clearInterval(audioReducer);
      }, 100);
    },
    playAgain() {
      this.audios.laugh.pause();
      this.winner = false;
      this.$confetti.stop();
      this.setUp();
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
.btn-principal {
  font-size: 2rem;
  -webkit-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  -moz-box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  box-shadow: 10px 10px 5px -5px rgba(135, 128, 135, 1);
  background: linear-gradient(150deg, #fdf9a6, #ff6c6c, #16ff22);
  background-size: 400% 400%;
  animation: BgGradient 10s ease infinite;
}
.middle-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 20%;
}
.player-container {
  height: 60vh;
}

@keyframes BgGradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>