<template>
  <div id="app">
    <DefaultLayout>
      <!-- Add fancy routing here -->
      <Home />
    </DefaultLayout>
  </div>
</template>

<script>
import DefaultLayout from './layouts/Default.vue';
import Home from './pages/Home.vue';

export default {
  components: {
    DefaultLayout,
    Home,
  },
};
</script>

<style lang="scss">
@import './scss/reset.scss';
@import './scss/base.scss';
</style>
