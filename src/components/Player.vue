<template>
  <div class="player">
    <input v-model="user.name" class="name" />
    <img :src="user.image" />
    <div class="container-points">
      <p>HP:</p>
      <input v-model="user.HP" class="points" />
    </div>
    <div class="container-points">
      <p>DPS:</p>
      <input v-model="user.DPS" class="points" />
    </div>
    <button @click="pickAndSetPlayer" class="btn">Randomize</button>
  </div>
</template>

<script>
export default {
  props: ["user", "pickAndSetPlayer"],
};
</script>

<style lang="scss">
@import "../scss/globalStyles.scss";
input {
  outline: none;
  border: 0px;
  border-radius: 20px;
  text-align: center;
  color: $font-color;
  background-color: rgba(0, 0, 0, 0);
}
.player {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  padding: 15px 0;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 50px;
  height: 100%;
  .name {
    font-size: 2rem;
  }
  img {
    max-width: 200px;
    max-height: 200px;
    border-radius: 100px;
  }
  .container-points {
    display: flex;
    font-size: 1.5rem;
    input {
      width: 80px;
      font-weight: 900;
      background-color: rgba(0, 0, 0, 0);
    }
    p {
      margin: 0;
    }
  }
  .btn {
    background-color: rgba(151, 255, 238, 0.75);
  }
}
</style>