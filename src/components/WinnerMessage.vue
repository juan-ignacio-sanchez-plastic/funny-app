<template>
  <div class="message">
    <p>
      {{
      winner === "TIE"
      ? "TIE"
      : `The winner is: ${winner}`
      }}
    </p>
  </div>
</template>

<script>
export default {
  props: ["winner"],
};
</script>
