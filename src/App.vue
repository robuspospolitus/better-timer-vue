<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import Timer from './components/Timer.vue';

let section = [
  {
    name: "naur",
    time: 20,
    additional_time: 2,
    color: "red",

  },
  {
    name: "rest",
    time: 10,
    additional_time: 0,
    color: "green",

  },
  {
    name: "lola",
    time: 30,
    additional_time: 1,
    color: "blue",

  },
  ];
</script>

<template>
  <header>
    <h1>Better Timer</h1>
  </header>

  <main id="inside">
    <Timer :sections="section"/>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  width: 100%;
  box-shadow: 0 -2px 8px  #00000080;
}

h1 {
  text-align: center;
  padding: 1rem;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
