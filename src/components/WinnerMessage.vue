<template>
  <div class="message">
    <p v-if="winner === 'TIE'" class="winner">TIE</p>
    <div v-else>
      <p>The winner is</p>
      <p class="winner">{{winner}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["winner"],
};
</script>

<style lang="scss">
.message {
  display: flex;
  flex-direction: column;
  justify-content: center;
  font-size: 2rem;
  margin-bottom: 100px;
  text-align: center;
  .winner {
    font-weight: 900;
    padding: 15px;
    border-radius: 50px;
    background-color: rgba(0, 0, 0, 0.5);
  }
}
</style>
