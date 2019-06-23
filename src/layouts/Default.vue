<template>
  <div>
    <Header />

    <div class="main">
      <slot />
    </div>
  </div>
</template>

<script>
import Header from '../components/Header.vue';

export default {
  components: {
    Header,
  },
};
</script>

<style lang="scss" scoped></style>
