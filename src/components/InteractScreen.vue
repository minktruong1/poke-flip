<template>
  <h1>Interact Screen</h1>
  <card-flip
    v-for="(card, index) in cardsContext"
    :key="index"
    :imgBackFaceUrl="`images/${card}.png`"
  />
</template>

<script>
import CardFlip from "./CardView.vue";

export default {
  props: {
    cardsContext: {
      type: Array,
      defalut: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
};
</script>
