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
  </div>
</template>

<script>
export default {
  props: ["user"],
};
</script>

<style lang="scss">
input {
  outline: none;
  border: 0px;
  border-radius: 20px;
  text-align: center;
}
.player {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  border-radius: 50px;
  height: 60vh;
  .name {
    font-size: 2rem;
  }
  img {
    max-width: 50%;
    max-height: 150px;
    border-radius: 400px;
  }
  .container-points {
    display: flex;
    font-size: 1.5rem;
    input {
      width: 80px;
      font-weight: 900;
    }
  }
}
</style>