<template>
  <Navbar />
  <router-view />
</template>

<script>
import Navbar from '@/components/Navbar.vue';

export default {
  components: {
    Navbar,
  },
};
</script>

<style lang="scss">
/** app styles **/
* {
  margin: 0;
  padding: 0;
}

#app {
  display: grid;
  grid-template-rows: auto 1fr auto;
}

html, body {
  height: 100vh;
}

body {
  background-color: rgb(247, 250, 252);
  color: rgb(26, 32, 44);
  font-family: 'Arial', serif;
  font-feature-settings: 'liga'; // Support for IE
  font-size: calc(1rem + 1vmin);
  font-style: normal;
  font-weight: 400;
  letter-spacing: normal;
  line-height: normal;
  text-rendering: optimizeLegibility; // Support for Safari and Chrome
  text-transform: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  /* browser scrollbar styles */
  &::-webkit-scrollbar {
    width: 0.75rem;
  }

  &::-webkit-scrollbar-track {
    background-color: rgb(247, 250, 252);
  }

  &::-webkit-scrollbar-thumb {
    border-radius: 0.5rem;
    background-color: rgb(26, 32, 44);
  }
}

a {
  color: inherit;
  text-decoration: none;
}

li {
  list-style: none;
}

main {
  flex-shrink: 0;
}

.footer {
  background: #edf2f7;
  bottom: 0;
  height: 3rem;
  position: fixed;
  width: 100%;

  .container {
    display: grid;
    place-items: center;

    .text-muted {
      color: #718096;
      font-size: 1rem;

      a:hover {
        color: #4a5568;
      }
    }
  }
}
</style>
