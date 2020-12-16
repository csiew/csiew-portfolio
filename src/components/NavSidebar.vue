<template>
  <div id="navSidebarMenu" class="vstack z-index-50 position-fixed anchor-left width-auto align-start noselect" v-bind:class="isVisible.sidebar === true ? 'height-full ' + (isMobile === true ? 'vstack-reverse anchor-bottom width-full' : 'nav-pill nav-pill-open nav-shadow nav-border-right anchor-top justify-space-between') : 'height-auto ' + (isMobile === true ? 'vstack-reverse anchor-bottom width-full' : 'nav-pill nav-shadow anchor-top')">
    <div class="z-index-200 hstack align-center" v-bind:class="isMobile === true ? 'nav nav-border-top justify-space-between width-full font-scale-xl' : 'nav-pill-header justify-start width-auto padding-xxs'">
      <button title="Toggle navigation menu" class="grid-item button-borderless cursor-pointer padding-s line-height-1 font-scale-l" v-on:click="isVisible.sidebar = !isVisible.sidebar">
        <span v-if="isVisible.sidebar === false">&#9776;</span>
        <span v-else>&#10005;</span>
      </button>
      <transition name="fade">
        <button v-show="helloIsVisible === false" class="grid-item button-hv-reveal cursor-pointer padding-xs line-height-1" v-bind:class="isMobile === true ? 'margin-s-right margin-auto-left font-scale-xxs' : 'margin-s-right margin-xs-left font-scale-s'" v-on:click="this.goToSection('top'); isVisible.sidebar = false">Back to top</button>
      </transition>
    </div>
    <div v-if="isVisible.sidebar === true" class="vstack height-full" v-bind:class="isMobile === true ? 'align-center justify-center width-full padding-none font-scale-l' : 'width-auto justify-space-between'">
      <div class="card-list overflow-y-auto" v-bind:class="isMobile === true ? 'width-full height-full padding-m-top padding-m-bottom' : ''">
        <ul v-bind:class="isMobile === true ? 'margin-auto-top margin-auto-bottom' : ''">
          <li v-for="item in this.sections" v-bind:key="item.id" v-show="!item.hide" class="cursor-pointer" v-bind:class="isMobile === true ? 'width-full padding-xs hstack align-center justify-center font-scale-l' : 'padding-m-left padding-s-right padding-xs-top padding-xs-bottom hstack align-start justify-center'" v-on:click="this.goToSection(item.id); isVisible.sidebar = false">
            {{ item.label }}
          </li>
        </ul>
      </div>
      <div v-if="isMobile === false" class="nav-border-top">
        <div class="width-max-240 margin-auto-top padding-s text-align-left font-scale-xs justify-end text-color-secondary">
          This portfolio site was built using Vue.js and is deployed on Netlify. It uses custom CSS utilities inspired by Bootstrap and Tailwind CSS.
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UserAgent from '@/utils/UserAgent.js';

export default {
  name: 'NavSidebar',
  props: {
    helloIsVisible: Boolean
  },
  data() {
    return {
      isVisible: {
        sidebar: false
      },
      state: {
        currentSectionIndex: 0
      },
      sections: [
        {
          id: 'top',
          label: 'Back to top',
          hide: true
        },
        {
          id: 'about',
          label: 'About Me'
        },
        {
          id: 'skills',
          label: 'Skills'
        },
        {
          id: 'qualities',
          label: 'Personal Qualities'
        },
        {
          id: 'edu',
          label: 'Education'
        },
        {
          id: 'experience',
          label: 'Professional Experience'
        },
        {
          id: 'techjourney',
          label: 'Technical Journey'
        },
        {
          id: 'projects',
          label: 'Projects'
        },
        {
          id: 'website',
          label: 'My Website'
        },
        {
          id: 'contact',
          label: 'Contact'
        }
      ]
    };
  },
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
    }
  },
  methods: {
    scrollToSection: function (sectionId) {
      try {
        const el = document.getElementById(sectionId);
        el && el.scrollIntoView({
          block: 'start',
          behavior: 'smooth'
        });
      } catch (err) {
        console.warn(err);
      }
    },
    goToSection: function (sectionId) {
      this.state.currentSectionIndex = this.sections.findIndex(x => x.id === sectionId);
      this.scrollToSection(sectionId);
    },
    nextSection: function () {
      if (this.state.currentSectionIndex < this.sections.length - 1) {
        this.state.currentSectionIndex += 1;
      }
      this.scrollToSection(this.sections[this.state.currentSectionIndex].id);
    },
    previousSection: function () {
      if (this.state.currentSectionIndex > 0) {
        this.state.currentSectionIndex -= 1;
      }
      this.scrollToSection(this.sections[this.state.currentSectionIndex].id);
    }
  }
}
</script>

<style scoped>
#navSidebarMenu {
  transition: var(--TRANSITION);
}

.nav-pill {
  background: var(--BG-COLOR);
  border-bottom-right-radius: 16px !important;
  animation: nav-pill-throbber 2s alternate-reverse infinite !important;
}

.nav-pill-header {
  width: 100% !important;
}
.nav-pill-header,
.nav-pill-header .button,
.nav-pill-header button {
  color: var(--BG-COLOR) !important;
  background: var(--ACCENT-PRIMARY-COLOR) !important;
  border-bottom-right-radius: 16px !important;
}

.nav-pill-open,
.nav-pill-open .nav-pill-header {
  border-bottom-right-radius: 0px !important;
}

@keyframes nav-pill-throbber {
  from {
    box-shadow: 0px 0px 8px var(--NAVBAR-SHADOW) !important;
  }
  to {
    box-shadow: 0px 0px 16px var(--NAVBAR-SHADOW) !important;
  }
}

button {
  transform: scale(1) translateY(0%);
  transition: var(--TRANSITION);
}
button:active {
  transform: scale(0.96) translateY(0%);
}

.grid-item {
  display: inline-flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}

.card-list li {
  border-radius: 0;
  font-family: var(--FONT-FAMILY-HEADER);
  transform: scale(1);
}
.card-list li:active {
  border-radius: var(--CARD-BORDER-RADIUS);
  transform: scale(0.96);
}
</style>