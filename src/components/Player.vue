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

<style>
</style>