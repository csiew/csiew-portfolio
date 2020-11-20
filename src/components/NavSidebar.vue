<template>
  <div class="z-index-200 position-fixed anchor-right width-auto noselect" v-bind:class="isVisible.sidebar === true ? (isMobile === true ? 'anchor-bottom width-full height-full' : 'anchor-top height-auto') : (isMobile === true ? 'anchor-bottom width-full height-auto' : 'anchor-topheight-auto')">
    <div class="vstack height-full align-end" v-bind:class="isMobile === true ? 'vstack-reverse' : ''">
      <div class="nav-translucent z-index-200 grid grid-auto-flow-column grid-gap-xl align-center padding-xs-top padding-xs-bottom" v-bind:class="isMobile === true ? 'nav nav-border-top justify-space-between width-full margin-none padding-m-left padding-m-right font-scale-xl' : 'card nav-shadow justify-end width-auto margin-s padding-s-left padding-s-right'">
        <button title="Back to top" class="button-borderless cursor-pointer padding-xs line-height-1 font-scale-s" v-on:click="this.goToSection('top'); isVisible.sidebar = false">
          TOP
        </button>
        <button title="Previous section" class="button-borderless cursor-pointer padding-xs line-height-1 font-scale-s" v-on:click="this.previousSection(); isVisible.sidebar = false">
          &uarr;
        </button>
        <button title="Next section" class="button-borderless cursor-pointer padding-xs line-height-1 font-scale-s" v-on:click="this.nextSection(); isVisible.sidebar = false">
          &darr;
        </button>
        <button title="Toggle navigation menu" class="button-borderless cursor-pointer padding-xs line-height-1 font-scale-s" v-on:click="isVisible.sidebar = !isVisible.sidebar">
          <span v-if="isVisible.sidebar === false">&#9776;</span>
          <span v-else>&#10005;</span>
        </button>
      </div>
      <transition v-bind:name="isMobile === true ? 'fade' : 'slide'">
        <div v-if="isVisible.sidebar === true" class="card-list nav-translucent width-full" v-bind:class="isMobile === true ? 'vstack align-center justify-center width-full height-full font-scale-xl' : 'card width-auto margin-s-right'">
          <ul>
            <li v-for="item in this.sections" v-bind:key="item.id" v-show="!item.hide" class="cursor-pointer" v-bind:class="isMobile === true ? 'width-full padding-s hstack align-center justify-center' : 'padding-m-left padding-m-right padding-xs-top padding-xs-bottom hstack align-end justify-center'" v-on:click="this.goToSection(item.id); isVisible.sidebar = false">
              {{ item.label }}
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import UserAgent from '@/utils/UserAgent.js';

export default {
  name: 'NavSidebar',
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
button {
  transform: scale(1) translateY(0%);
  transition: var(--TRANSITION);
}
button:hover {
  color: var(--ACCENT-PRIMARY-COLOR) !important;
  transform: scale(1.125) translateY(-5%);
}
button:active {
  transform: scale(0.96) translateY(0%);
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

.slide-enter-active,
.slide-leave-active {
  transition: all 0.25s ease-in-out;
}
.slide-enter-from,
.slide-leave-to {
  opacity: 0;
  transform: translate(50%, -75%) scale(0) skewX(-10deg);
}
.slide-enter-to {
  opacity: 1;
  transform: translateY(0%, 0%) scale(1) skewX(0deg);
}
</style>