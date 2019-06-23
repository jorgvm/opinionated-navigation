<template>
  <div
    class="navigation"
    :class="[menuActive ? 'active' : 'inactive', pageTopStyleActive ? 'top-style' : 'body-style']"
  >
    <ul class="list">
      <li
        class="item"
        :class="title === itemActive ? 'active' : ''"
        v-for="title in items"
        :key="title"
      >
        <a @click.prevent="handleClick">
          {{ title }}
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import VueTypes from 'vue-types';

export default {
  data() {
    return {
      itemActive: 'officiis',
      items: ['commodi', 'placeat', 'officiis', 'repudiandae', 'similique', 'adipisci'],
    };
  },
  props: {
    menuActive: VueTypes.bool.def(false),
    pageTopStyleActive: VueTypes.bool.def(false),
  },

  methods: {
    handleClick() {
      // Routing here.
      this.$emit('click');
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../scss/params.scss';

.navigation {
  position: relative;
  display: flex;
  align-items: center;
  transition: opacity 0.3s;
}

.list {
  display: none;
  text-transform: uppercase;
}

.item {
  margin: 10px 0;

  a {
    cursor: pointer;
    @include stretchline($reversed: true);
  }

  &.active a {
    @include stretchline;
  }
}

.navigation.active {
  .item:hover {
    color: white;
  }
}

@media (max-width: 1023px) {
  .navigation.active {
    background: $bgr-primary;
    background-size: cover;
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    padding: 50px 10px 10px 7px;

    .list {
      display: block;
    }
  }

  .navigation.inactive {
    opacity: 0;
  }
}

@media (min-width: 1024px) {
  .navigation {
    margin-left: auto;
  }

  .list {
    display: flex;
  }

  .item:not(:last-child) {
    margin-right: 20px;
  }

  .navigation.body-style {
    opacity: 0;
  }

  .navigation.active {
    opacity: 1;
  }
}
</style>
