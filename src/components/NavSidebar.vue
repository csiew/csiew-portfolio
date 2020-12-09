<template>
  <div id="navSidebarMenu" class="z-index-50 position-fixed anchor-right width-auto noselect" v-bind:class="isVisible.sidebar === true ? (isMobile === true ? 'anchor-bottom width-full height-full' : 'anchor-top height-auto margin-s-top margin-l-right') : (isMobile === true ? 'anchor-bottom width-full height-auto' : 'anchor-top height-auto margin-s-top margin-l-right')">
    <div class="vstack height-full align-end" v-bind:class="isMobile === true ? 'vstack-reverse' : 'card nav-translucent'">
      <div class="z-index-200 hstack align-center" v-bind:class="isMobile === true ? 'nav nav-border-top justify-space-between width-full font-scale-xl' : 'justify-end width-auto'">
        <transition name="fade">
          <img v-show="helloIsVisible === false" class="grid-item profile-img profile-img-xs nav-shadow cursor-pointer border-radius-100pct margin-auto-vertical padding-none nodrag noselect" v-bind:class="isMobile === true ? 'margin-s-left margin-auto-right' : 'margin-s-left margin-xs-right'" alt="profile" title="Back to top" src="@/assets/images/profile_0.jpg" v-on:click="this.goToSection('top'); isVisible.sidebar = false" />
        </transition>
        <button title="Toggle navigation menu" class="grid-item button-borderless cursor-pointer padding-s line-height-1 font-scale-l" v-bind:class="isMobile === true ? 'margin-auto-left' : ''" v-on:click="isVisible.sidebar = !isVisible.sidebar">
          <span v-if="isVisible.sidebar === false">&#9776;</span>
          <span v-else>&#10005;</span>
        </button>
      </div>
      <transition name="fade">
        <div v-if="isVisible.sidebar === true" v-bind:class="isMobile === true ? 'nav vstack align-center justify-center width-full height-full padding-none font-scale-l' : 'nav-transparent nav-border-top width-auto'">
          <div class="card-list overflow-y-auto" v-bind:class="isMobile === true ? 'width-full height-full padding-m-top padding-m-bottom' : ''">
            <ul v-bind:class="isMobile === true ? 'margin-auto-top margin-auto-bottom' : ''">
              <li v-for="item in this.sections" v-bind:key="item.id" v-show="!item.hide" class="cursor-pointer" v-bind:class="isMobile === true ? 'width-full padding-xs hstack align-center justify-center font-scale-l' : 'padding-m-left padding-s-right padding-xs-top padding-xs-bottom hstack align-end justify-center'" v-on:click="this.goToSection(item.id); isVisible.sidebar = false">
                {{ item.label }}
              </li>
            </ul>
          </div>
          <div v-if="isMobile === false" class="nav-border-top">
            <div class="padding-s text-color-secondary" v-bind:class="isMobile === false ? 'width-max-240 margin-auto-left padding-xs-top padding-xs-bottom text-align-right font-scale-xs' : 'text-align-center font-scale-xxs'">
              This portfolio site was built using Vue.js and is deployed on Netlify. It uses custom CSS utilities inspired by Bootstrap and Tailwind CSS.
            </div>
          </div>
        </div>
      </transition>
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
  box-shadow: var(--NAVBAR-SHADOW);
  transition: var(--TRANSITION);
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