<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: "HomeView",
};
</script>

<style lang="scss">
#app {
  max-height: 100%;
  position: relative;
  height: 100%;
  max-width: 1250px;
  margin: 0 auto;
}

* {
  margin: 0;
  padding: 0;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
html {
  height: 100%;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  transition: background 0.25s ease-in-out;
  min-height: 100%;
  height: 100%;
}

html {
  overflow-y: auto !important;

  &::-webkit-scrollbar {
    background-color: black !important;
    -webkit-appearance: none !important;
    width: 4px !important;
  }

  &::-webkit-scrollbar-thumb {
    border-radius: 2px !important;
    background-color: #e3e1ef !important;
  }
}
</style>
