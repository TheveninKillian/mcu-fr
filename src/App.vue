<template>
  <div id="app">
    <div id="overlay" class="overlay"></div>
    <Header />
    <section id="timeline" class="timeline">
      <div class="container" id="container-timeline">
        <Item
          v-for="item in marvel"
          :key="item.id"
          :id="item.id"
          :name="item.name"
          :date="item.date"
          :description="item.description"
          :logo="item.logo"
          :alt="item.alt"
          :visibleCard.sync="item.visibleCard"
        />
      </div>
    </section>
  </div>
</template>

<script>
import Header from "./components/Header";
import Item from "./components/Item";
import Data from "./assets/data";

export default {
  name: "App",
  components: {
    Header,
    Item
  },
  data() {
    return {
      Data,
      marvel: []
    };
  },
  beforeMount() {
    this.marvel = [
      ...this.Data.phase_1,
      ...this.Data.phase_2,
      ...this.Data.phase_3,
      ...this.Data.phase_4
    ];
  }
};
</script>

<style lang="scss">
@import "./scss/variables";
@import "./scss/other";

* {
  box-sizing: border-box;
}

html,
body {
  overflow-x: hidden;
}

body {
  background-color: #212121;

  font-family: "Roboto Condensed", sans-serif;
}

.timeline {
  position: relative;
  color: $text;
  margin-top: 20px;

  &::before {
    content: "";
    background: $light-primary;
    width: 2px;
    height: 100%;
    position: absolute;
    left: 20px;
    transform: translateX(-50%);
  }
}

.container {
  margin-right: auto;
  margin-left: auto;

  &::before {
    display: table;
    content: " ";
  }
}

.overlay {
  background-color: rgba(0, 0, 0, 0.7);
  position: fixed;
  top: 0;
  left: 0;

  width: 100%;
  height: 100%;
  z-index: 50;
  display: none;
}

.text-hidden {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

@import "./scss/responsive";
</style>
