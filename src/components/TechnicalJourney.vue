<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-bottom">
      <h2>Technical Journey</h2>
    </div>
    <div class="grid grid-col-1 grid-gap-l width-full width-max-1024 margin-auto-horizontal">
      <div id="techJourneyTooltip" v-show="isVisible.tooltip === true" class="tooltip"></div>
      <div class="vstack width-full align-center justify-center margin-auto-horizontal padding-l-top padding-m-left padding-m-right">
        <div class="timeline width-full margin-l-left margin-l-right" v-on:mousemove="this.setTooltipPosition" v-on:mouseout="this.hideTooltip()">
          <button v-for="pointId in Object.keys(this.sections)" v-bind:key="`timeline-point-${pointId}`" class="timeline-point" v-bind:class="(Number(pointId) === state.currentSectionId) ? 'button-selected timeline-point-selected' : ''" v-on:mouseover="this.showTooltip(Number(pointId))" v-on:mouseout="this.hideTooltip()" v-on:click="this.setCurrentSection(Number(pointId))"></button>
        </div>
        <div class="hstack hstack-space-between align-center margin-xl-top">
          <button class="button-borderless cursor-pointer flex-inline flex-flow-row-wrap align-center justify-center padding-none line-height-1 font-scale-l" v-on:click="this.setCurrentSectionPrevious()">
            <span>&#10229;</span>
            <span class="padding-s">Previous</span>
          </button>
          <button class="button-borderless cursor-pointer flex-inline flex-flow-row-wrap-reverse align-center justify-center padding-none line-height-1 font-scale-l" v-on:click="this.setCurrentSectionNext()">
            <span class="padding-s">Next</span>
            <span>&#10230;</span>
          </button>
        </div>
      </div>
      <hr class="margin-l-top margin-none-bottom" />
      <transition name="fade">
        <div v-show="isVisible.timelineSection === true" class="grid grid-gap-xl align-center justify-center width-full margin-auto-horizontal padding-m" v-bind:class="this.sections[state.currentSectionId].image ? 'grid-col-auto-fill-320' : 'grid-col-1'">
          <div v-if="this.sections[state.currentSectionId].image && this.sections[state.currentSectionId].image.align === 'left'" class="grid-item vstack align-center justify-center">
            <img v-bind:src="getImage(this.sections[state.currentSectionId].image.src)" class="grid-item card card-img width-full aligh-center justify-center noselect nodrag" v-bind:alt="this.sections[state.currentSectionId].image.alt" />
            <div v-if="this.sections[state.currentSectionId].image.caption" class="caption width-full padding-s">{{ this.sections[state.currentSectionId].image.caption }}</div>
          </div>
          <div class="grid-item flex-inline flex-flow-column aligh-center justify-center">
            <h3 class="margin-xs-top margin-s-bottom font-scale-xl">{{ this.sections[state.currentSectionId].title }}</h3>
            <div v-for="contentParagraph in this.sections[state.currentSectionId].content" v-bind:key="contentParagraph[0] + contentParagraph.length" v-html="this.formatMarkdown(contentParagraph)"></div>
          </div>
          <div v-if="this.sections[state.currentSectionId].image && this.sections[state.currentSectionId].image.align === 'right'" class="grid-item vstack align-center justify-center">
            <img v-bind:src="getImage(this.sections[state.currentSectionId].image.src)" class="grid-item card card-img width-full aligh-center justify-center noselect nodrag" v-bind:alt="this.sections[state.currentSectionId].image.alt" />
            <div v-if="this.sections[state.currentSectionId].image.caption" class="caption width-full margin-xs-top padding-s">{{ this.sections[state.currentSectionId].image.caption }}</div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
import UserAgent from '@/utils/UserAgent.js';

export default {
  name: 'TechnicalJourney',
  data() {
    return {
      isVisible: {
        tooltip: true,
        timelineSection: false
      },
      state: {
        currentSectionId: 0,
        timelinePointHoverId: 0
      },
      tooltipValue: '',
      sections: {
        0: {
          title: 'The Beginning',
          content: [
            'I have a long-standing love for software development and user experience design from a young age.',
            'At the age of 9, I started building interactive prototypes of desktop environments in PowerPoint.'
          ],
          image: {
            src: 'powerpoint_2003.png',
            alt: 'PowerPoint 2003',
            align: 'right',
            caption: 'Microsoft PowerPoint 2003'
          }
        },
        1: {
          title: 'Expanding Horizons',
          content: [
            'From the ages of 13 to 18, I expanded my work into actual operating systems.',
            'During this time, I built illume OS and Antorca Linux. Both of which aimed to provide a minimalist and easy-to-use desktop experience for users who were looking into recycling old hardware.',
            'I also started to get into basic web development and Python scripting to help create some utilities here and there.'
          ],
          image: {
            src: 'projects/antorca_linux.png',
            alt: 'Antorca Linux',
            align: 'left',
            caption: 'Antorca Linux desktop'
          }
        },
        2: {
          title: 'Full-stack Web Development',
          content: [
            'Through numerous uni assignments and projects, I delved into full stack development. I learned to work with the Spring Framework for backend services, and with React for web frontends. I also used MySQL and PostgreSQL for these projects.',
            'Through my personal projects I have also been working with the Node.js runtime and Express.js framework for backend services, and Vue.js for web frontends. I have experimented with creating my own UI library for one of these projects in place of React or Vue.'
          ],
          image: {
            src: 'projects/cast.png',
            alt: 'Cast',
            align: 'right',
            caption: 'Cast podcast web app, originally built with my own custom frontend UI library but is currently being rewritten using Vue.js.'
          }
        },
        3: {
          title: 'Scripting and Mobile App Development',
          content: [
            'Along the way, I picked up numerous handy skills such as working with Python on larger-scale projects. I have used Python in a range of projects with something as simple as a basic content management system for my personal blog, to something with moderate complexity like a command-line podcast player, to something as complicated as a whole window manager (or desktop environment) for Linux.',
            'I have also done some mobile app development. For a university assignment, I formulated, design, and built an iOS app that let users report incidents and delays on Melbourne\'s public transport network (it used the PTV APIs for the relevant data; it also used Google Firebase to store user data and perform authentication).'
          ],
          image: {
            src: 'projects/biscuitwm.png',
            alt: 'BiscuitWM',
            align: 'left',
            caption: 'BiscuitWM, a minimalist window manager for Linux-based operating systems written in Python.'
          }
        }
      }
    }
  },
  methods: {
    formatMarkdown: function (content) {
      return marked(content, { sanitized: true });
    },
    getImage: function (imageUrl) {
      try {
        return require(`@/assets/images/${imageUrl}`);
      } catch (err) {
        console.warn(err);
        return;
      }
    },
    setCurrentSection: function (sectionId) {
      if (this.state.currentSectionId !== sectionId) {
        this.isVisible.timelineSection = false;
        setTimeout(() => {
          this.state.currentSectionId = sectionId;
          this.isVisible.timelineSection = true;
        }, 500);
      }
    },
    setCurrentSectionPrevious: function () {
      let sectionKeys = Object.keys(this.sections);
      if (sectionKeys.includes(`${this.state.currentSectionId}`)) {
        if (Number(this.state.currentSectionId) <= 0) {
          this.setCurrentSection(sectionKeys.length - 1);
        } else {
          this.setCurrentSection(Number(this.state.currentSectionId) - 1);
        }
      } else {
        console.log("Invalid index");
      }
    },
    setCurrentSectionNext: function () {
      let sectionKeys = Object.keys(this.sections);
      if (sectionKeys.includes(`${this.state.currentSectionId}`)) {
        if (Number(this.state.currentSectionId) >= sectionKeys.length - 1) {
          this.setCurrentSection(0);
        } else {
          this.setCurrentSection(Number(this.state.currentSectionId) + 1);
        }
      } else {
        console.log("Invalid index");
      }
    },
    showTooltip: function (sectionId) {
      if (UserAgent.isMobile() === false) {
        document.getElementById('techJourneyTooltip').innerHTML = this.sections[sectionId].title;
        this.isVisible.tooltip = true;
      }
    },
    hideTooltip: function () {
      this.isVisible.tooltip = false;
    },
    setTooltipPosition: function (e) {
      e = e || window.event;
      e.preventDefault();
      var tooltipHeight, tooltipWidth, isNearViewBottom, isNearViewLeft, isNearViewRight;
      const tooltip = document.getElementById('techJourneyTooltip');
      try {
        tooltipHeight = tooltip.offsetHeight;
        tooltipWidth = tooltip.offsetWidth;
      } catch (err) {
        tooltipHeight = 0;
        tooltipWidth = 0;
      }
      isNearViewBottom = (window.innerHeight - e.clientY < tooltipHeight * 1.75) ? true : false;
      isNearViewLeft = (e.clientX < tooltipWidth * 1.75) ? true : false;
      isNearViewRight = (window.innerWidth - e.clientX < tooltipWidth * 1.75) ? true : false;
      if (tooltip) {
        tooltip.style.top = (isNearViewBottom === false ? e.clientY + 25 : e.clientY - tooltipHeight + 25) + 'px';
        tooltip.style.left = (isNearViewRight === false ? (isNearViewLeft === false ? e.clientX - (tooltipWidth / 2) : tooltipWidth * 0.75) : e.clientX - tooltipWidth) + 'px';
      }
    }
  },
  mounted: function () {
    this.setCurrentSection(0);
    this.isVisible.timelineSection = true;
  }
}
</script>

<style scoped>
.grid-item {
  align-self: flex-start;
}
</style>