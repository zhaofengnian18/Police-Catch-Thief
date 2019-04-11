<template>
  <div id="app">
    <Start :class="{hidden:startHidden}" @startgame="startgame"/>
    <CLine
      v-if="!clineHidden"
      ref="Line"
      :row="3"
      :col="30"
      @backToMain="backToMain"
      :difficulty="difficulty"
    />
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import CLine from "./components/line";
import Start from "./components/start";

export default {
  name: "app",
  components: {
    CLine,
    Start
  },
  data() {
    return {
      startHidden: false,
      clineHidden: true,
      difficulty: ""
    };
  },
  methods: {
    startgame: function(difficulty) {
      this.startHidden = true;
      this.clineHidden = false;
      this.difficulty = difficulty;
    },
    backToMain: function() {
      this.startHidden = false;
      this.clineHidden = true;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.hidden {
  display: none !important;
}
</style>
