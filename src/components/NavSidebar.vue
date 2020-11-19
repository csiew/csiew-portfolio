<template>
  <div class="position-fixed anchor-right width-auto noselect" v-bind:class="isVisible.sidebar === true ? (isMobile === true ? 'z-index-200 width-full height-full' : 'z-index-200 height-auto') : (isMobile === true ? 'width-full height-auto' : 'height-auto')">
    <div class="vstack height-full align-end">
      <div class="nav-translucent z-index-200 grid grid-auto-flow-column grid-gap-xl align-center justify-end padding-s" v-bind:class="isMobile === true ? 'nav nav-border-bottom width-full margin-none font-scale-xl' : 'card width-auto margin-s'">
        <button title="Back to top" class="button-borderless cursor-pointer padding-none line-height-1 font-scale-s" v-on:click="this.goToSection('top')">
          &UpArrowBar;
        </button>
        <button title="Previous section" class="button-borderless cursor-pointer padding-none line-height-1 font-scale-s" v-on:click="this.previousSection()">
          &uarr;
        </button>
        <button title="Next section" class="button-borderless cursor-pointer padding-none line-height-1 font-scale-s" v-on:click="this.nextSection()">
          &darr;
        </button>
        <button title="Toggle navigation menu" class="button-borderless cursor-pointer padding-none line-height-1 font-scale-s" v-on:click="isVisible.sidebar = !isVisible.sidebar">
          <span v-if="isVisible.sidebar === false">&#9776;</span>
          <span v-else>&#10005;</span>
        </button>
      </div>
      <transition v-bind:name="isMobile === true ? 'fade' : 'slide'">
        <div v-if="isVisible.sidebar === true" class="card-list width-full" v-bind:class="isMobile === true ? 'nav-translucent vstack align-center justify-center width-full height-full font-scale-xl' : 'card width-auto margin-s-right'">
          <ul>
            <li v-for="item in this.sections" v-bind:key="item.id" v-show="!item.hide" class="cursor-pointer" v-bind:class="isMobile === true ? 'width-full padding-s hstack align-center justify-center' : 'padding-l-left padding-m-right padding-xs-top padding-xs-bottom hstack align-end justify-center'" v-on:click="this.goToSection(item.id); isVisible.sidebar = false">
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
      } else {
        this.state.currentSectionIndex = 0;
      }
      this.scrollToSection(this.sections[this.state.currentSectionIndex].id);
    },
    previousSection: function () {
      if (this.state.currentSectionIndex > 0) {
        this.state.currentSectionIndex -= 1;
      } else {
        this.state.currentSectionIndex = this.sections.length - 1;
      }
      this.scrollToSection(this.sections[this.state.currentSectionIndex].id);
    }
  }
}
</script>

<style scoped>
.card-list li {
  font-family: var(--FONT-FAMILY-HEADER);
}
.card-list li:active {
  filter: brightness(0.125) contrast(0.125);
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