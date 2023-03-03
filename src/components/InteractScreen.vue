<template>
  <div class="screen">
    <h1>Interact Screen</h1>
    <card-flip
      v-for="(card, index) in cardsContext"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index, value: card }"
      @onflip="checkRule($event)"
    />
  </div>
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
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Correct");
        // Make disable cards
        this.$refs[`card-${this.rules[0].index}`][0].onTurnDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onTurnDisableMode();
        // Reset rules[]
        this.rules = [];
        const disableElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disableElements &&
          disableElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 1000);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        // 2 cards face-down
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          console.log("Wrong");
          // reset rules[]
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>
