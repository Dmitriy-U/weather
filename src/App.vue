<template>
    <div id="app">
        <Home/>
    </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home,
  },
};
</script>

<style lang="scss">
    @import '../node_modules/bootstrap/scss/bootstrap.scss';
    @import '../node_modules/bootstrap-vue/src/index.scss';

    #app {
        height: 100%;
        overflow: auto;
    }
</style>
