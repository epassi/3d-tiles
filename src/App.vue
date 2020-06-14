<template>
  <div id="app">
    <div class="tile-grid">
      <tile
        v-for="(item, i) in items"
        :label="item.label"
        :color="item.color"
        :key="i"
      ></tile>
    </div>
  </div>
</template>

<script>
import Tile from "./components/tile.vue";

export default {
  name: "App",

  data() {
    return {
      items: [
        { label: "not", color: "#2d00f7" },
        { label: "sure", color: "#6a00f4" },
        { label: "where", color: "#8900f2" },
        { label: "i", color: "#a100f2" },
        { label: "am", color: "#b100e8" },
        { label: "going", color: "#bc00dd" },
        { label: "but", color: "#d100d1" },
        { label: "it’s", color: "#db00b6" },
        { label: "ok", color: "#e500a4" }
      ]
    };
  },

  components: {
    Tile
  }
};
</script>

<style lang="scss">
:root {
  font-family: sans-serif;
  font-size: 62.5%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 100vw;
  height: 100vh;
  background: #252525;
  overflow: auto;
}

.tile-grid {
  height: 100%;
  margin: 3rem;

  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: min-content;
  grid-gap: 3rem;
}
</style>
