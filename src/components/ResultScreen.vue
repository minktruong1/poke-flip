<template>
  <div class="screen">
    <h1>Finish!!!</h1>
    <p>{{ Math.round(timer / 1000) }} seconds</p>
    <button @click="onPlayAgain">Play again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
      required: true,
    },
  },
  methods: {
    onPlayAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>
