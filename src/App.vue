<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'inmatch'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right v-if="statusMatch === 'default'" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyrightScreen.vue";

import { shuffler } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        timerCheck: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  methods: {
    onHandleBeforeStart(config) {
      console.log("Runing.....", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCardList = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );

      const secondCardList = [...firstCardList];
      const cards = [...firstCardList, ...secondCardList];
      this.settings.cardsContext = shuffler(shuffler(shuffler(cards)));
      this.settings.timerCheck = new Date().getTime();
      //data ready
      this.statusMatch = "inmatch";
    },
    onGetResult() {
      //get Time
      this.timer = new Date().getTime() - this.settings.timerCheck;
      //go to result screen
      this.statusMatch = "result";
    },
  },
};
</script>
