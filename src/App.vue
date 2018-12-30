<template>
  <div id="app" class="container">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    App vue template
    <MainView msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MainView from './components/MainView.vue';

@Component({
  components: {
    MainView,
  },
})
export default class App extends Vue {}
</script>

<style>

</style>
