<template>
    <div id="app">
        <leftMenu />
        <calendar />
    </div>
</template>

<script>
// import components
import leftMenu from './components/left-menu';
import calendar from './components/calendar';


export default {
  name: 'App',
  components: { 
    leftMenu,
    calendar,
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  width: 100%;
  height: 100%;
  flex: auto;
}

</style>
