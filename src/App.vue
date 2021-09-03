<template>
  <div id="app">
    <Header />
    <DailyMealsBlock />
    <NewMealsBlock />
  </div>
</template>

<script>
    import Header from './components/Headers/Header.vue';
    import DailyMealsBlock from './components/DailyMeals/DailyMealsBlock.vue';
    import NewMealsBlock from './components/NewMeals/NewMealsBlock.vue';

    export default {
      name: 'App',
      components: {
        Header,
        DailyMealsBlock,
        NewMealsBlock
      }
    }
</script>

<style>
    body {
        background-color: grey;
    }
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
    }
</style>
