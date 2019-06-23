<template>
  <div class="wrap">
    <div
      class="header"
      :class="[
        scrolledUp ? 'scrolled-up' : '',
        menuActive ? 'active' : 'inactive',
        pageTopStyleActive ? 'top-style' : 'body-style',
      ]"
      ref="header"
    >
      <div class="site-logo">
        Initech
      </div>

      <Navigation
        :pageTopStyleActive="pageTopStyleActive"
        :menuActive="menuActive"
        v-on:click="menuActive = false"
      />

      <button
        @click="menuActive = !menuActive"
        class="toggle"
        :aria-label="menuActive ? 'close menu' : 'open menu'"
      >
        <svg
          width="32px"
          height="32px"
          viewBox="0 0 32 32"
          v-html="menuActive ? iconCross : iconHamburger"
        />
      </button>
    </div>

    <div class="header-image" ref="scrollBreakpoint">
      <img :src="imageHome" alt="Record Store" />
    </div>
  </div>
</template>

<script>
import Navigation from './Navigation.vue';
import imageHome from '../assets/bgr-home.jpg';

export default {
  data() {
    return {
      imageHome,
      iconHamburger:
        '<path d="M4,10h24c1.104,0,2-0.896,2-2s-0.896-2-2-2H4C2.896,6,2,6.896,2,8S2.896,10,4,10z M28,14H4c-1.104,0-2,0.896-2,2  s0.896,2,2,2h24c1.104,0,2-0.896,2-2S29.104,14,28,14z M28,22H4c-1.104,0-2,0.896-2,2s0.896,2,2,2h24c1.104,0,2-0.896,2-2  S29.104,22,28,22z"/>',
      iconCross:
        '<polygon id="Shape" points="28 2.83 25.17 0 14 11.17 2.83 1.77635684e-15 0 2.83 11.17 14 1.77635684e-15 25.17 2.83 28 14 16.83 25.17 28 28 25.17 16.83 14"></polygon>',
      lastPositionY: 0,
      pageTopStyleActive: true,
      menuActive: false,
      scrolledUp: false,
    };
  },

  components: {
    Navigation,
  },

  computed: {},

  methods: {
    handleScroll() {
      const doc = document.documentElement;
      const windowOffsetY = (window.pageYOffset || doc.scrollTop) - (doc.clientTop || 0);
      const windowWidth = window.innerWidth;
      const breakpoint = this.$refs.scrollBreakpoint;
      const offsetBottom = breakpoint.offsetTop + breakpoint.offsetHeight;
      const headerHeight = this.$refs.header.offsetHeight;
      const scrolledPast = windowOffsetY < this.lastPositionY && windowOffsetY > offsetBottom;

      // Check if user scrolled up
      if (windowWidth >= 1024) {
        this.menuActive = scrolledPast;
      } else {
        this.scrolledUp = scrolledPast;
      }

      // Check if the user scroll past the header image.
      // Calculate window height + half of the header
      // Check if that's more than the 'bottom offset' of the image
      if (windowOffsetY + headerHeight / 2 > offsetBottom) {
        this.pageTopStyleActive = false;
      } else {
        this.pageTopStyleActive = true;
      }

      // Set new offset
      this.lastPositionY = windowOffsetY;
    },
  },

  created() {
    window.addEventListener('scroll', this.handleScroll);
  },

  destroyed() {
    window.removeEventListener('scroll', this.handleScroll);
  },
};
</script>

<style lang="scss" scoped>
@import '../scss/params.scss';

.header {
  position: fixed;
  padding: 0 5px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(0, 0, 0, 0);
  width: 100%;
  height: 50px;
  color: white;

  &:before {
    content: '';
    transition: all 0.3s;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    background: $bgr-primary;
    background-size: cover;
  }
}

.header-image {
  max-height: 420px;
  overflow: hidden;

  img {
    width: 100%;
  }
}

.site-logo {
  margin-right: 5vw;
  color: white;
  font-size: 40px;
  font-weight: bold;
  font-style: italic;
  text-transform: uppercase;
  transition: color 0.3s;
  z-index: 1;
}

.header.body-style {
  .site-logo {
    color: black;
  }
}

.toggle {
  width: 30px;
  transition: opacity 0.3s;
  margin: 0 20px 0 30px;
  z-index: 1;

  svg {
    width: 20px;
  }
}

@media (max-width: 1023px) {
  .toggle svg {
    fill: white;
  }

  .header {
    transition: transform 0.6s;
  }

  .header.body-style {
    &:before {
      opacity: 1;
    }

    .site-logo {
      color: white;
    }
  }

  .header.inactive.body-style {
    transition: all 0s;
    transform: translateY(-100%);
  }

  .header.scrolled-up.body-style {
    transition: all 0.3s;
    transform: translateY(0);
  }
}

@media (min-width: 1024px) {
  .header {
    height: 80px;
    padding: 10px;
  }

  .toggle:hover {
    animation: shake 1s infinite;
  }

  .header.active {
    &:before {
      opacity: 1;
    }

    .site-logo {
      color: white;
    }

    .toggle svg {
      fill: white;
    }
  }

  .header.top-style .toggle {
    opacity: 0;
  }
}
</style>
