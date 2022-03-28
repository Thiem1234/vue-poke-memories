<template>
  <app-main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  ></app-main-screen>
  <interact-screen
    @onFinish="onGetResult()"
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
  ></interact-screen>
  <result-screen
    @onStartAgain="statusMatch = 'default'"
    :timer="timer"
    v-if="statusMatch === 'result'"
  ></result-screen>
  <copy-right></copy-right>
</template>
<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array.js";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";
export default {
  data: function () {
    return {
      statusMatch: "default",
      timer: 0,
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
    };
  },
  components: {
    appMainScreen: MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  methods: {
    onHandleBeforeStart: function (config) {
      this.statusMatch = "match";
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      console.log(firstCards);
      const secondCards = [...firstCards];
      console.log(secondCards);
      const cards = [...firstCards, ...secondCards];
      console.log(cards);
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      console.log(this.settings.cardsContext);
      this.settings.startedAt = new Date().getTime();

      // data ready
      console.log("Running handle before start ", config);
    },
    onGetResult: function () {
      // Get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      // switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
<style></style>
