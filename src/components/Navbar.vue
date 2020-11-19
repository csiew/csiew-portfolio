<template>
  <div id="navbar" class="nav nav-border-bottom navbar vstack justify-stretch align-stretch width-full">
    <div class="hstack padding-xs" v-bind:class="!this.isMobile() ? 'justify-center' : 'justify-end'">
      <div v-if="!this.isMobile()" class="hstack width-auto align-center">
        <small>
          <div v-if="isVisible.navLinks === true" class="tabbar">
            <router-link v-for="item in this.pages" v-bind:key="item.id" class="tabbar-item" v-bind:to="item.route">{{ item.label }}</router-link>
          </div>
        </small>
      </div>
      <div v-else>
        <button class="button-borderless padding-none-top padding-none-bottom line-height-1 font-scale-xl" v-on:click="this.toggleNavMenu()">&#9776;</button>
      </div>
    </div>
  </div>
  <transition name="fade">
    <div v-if="isVisible.navMenu === true" id="navbarMenu" class="nav nav-translucent navmenu vstack width-full text-align-left">
      <div class="flex-inline flex-flow-column height-full align-center justify-start">
        <div class="width-full">
          <div class="hstack justify-end padding-s">
            <button class="button-borderless padding-none-top padding-none-bottom line-height-1 font-scale-xl" v-on:click="this.hideNavMenu()">&#10005;</button>
          </div>
          <div class="vstack padding-m">
            <router-link v-for="item in this.pages" v-bind:key="item.id" class="tabbar-vertical-item text-align-center font-scale-xl" v-bind:to="item.route" v-on:click="this.hideNavMenu()">{{ item.label }}</router-link>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Navbar',
  data() {
    return {
      isVisible: {
        navLinks: true,
        navMenuButton: false,
        navMenu: false
      },
      pages: [
        {
          id: 'home',
          label: 'Home',
          route: '/'
        },
        {
          id: 'about',
          label: 'About',
          route: '/about'
        },
        {
          id: 'projects',
          label: 'Projects',
          route: '/projects'
        }
      ]
    };
  },
  methods: {
    isMobile() {
      if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        return true
      } else {
        return false
      }
    },
    revealNavMenuButton() {
      this.isVisible.navMenuButton = true;
    },
    hideNavMenuButton() {
      this.isVisible.navMenuButton = false;
    },
    revealNavMenu() {
      this.isVisible.navMenu = true;
    },
    hideNavMenu() {
      this.isVisible.navMenu = false;
    },
    toggleNavMenu() {
      this.isVisible.navMenu = !this.isVisible.navMenu;
    },
    revealNavLinks() {
      this.isVisible.navLinks = true;
    },
    hideNavLinks() {
      this.isVisible.navLinks = false;
    },
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.125s ease-in-out;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0%;
  filter: saturate(0%) blur(5px) drop-shadow(0px 4px 8px rgba(0,0,0,0.5));
}
.fade-enter-to {
  opacity: 100%;
  filter: saturate(100%) blur(0px) drop-shadow(none);
}
</style>