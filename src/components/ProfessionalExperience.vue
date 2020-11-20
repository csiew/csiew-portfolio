<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-bottom">
      <h2>Professional Experience</h2>
    </div>
    <div class="grid grid-col-1 grid-gap-l width-full width-max-1024 margin-auto-horizontal">
      <div id="proExpTooltip" v-show="isVisible.tooltip === true" class="tooltip"></div>
      <div class="vstack width-full align-center justify-center margin-auto-horizontal padding-l-top padding-m-left padding-m-right">
        <div class="timeline width-full margin-l-left margin-l-right" v-on:mousemove="this.setTooltipPosition" v-on:mouseout="this.hideTooltip()">
          <button v-for="item in this.experience" v-bind:key="item.id" class="timeline-point" v-bind:class="(item.id === state.currentSectionId) ? 'button-selected timeline-point-selected' : ''" v-on:mouseover="this.showTooltip(item.id)" v-on:mouseout="this.hideTooltip()" v-on:click="this.setCurrentSection(item.id)"></button>
        </div>
        <div class="hstack hstack-space-between align-center margin-xl-top">
          <button class="button-hv-reveal cursor-pointer flex-inline flex-flow-row align-center justify-center" v-on:click="this.setCurrentSectionPrevious()">
            <span class="margin-xs-right">&#10094;</span>
            <span>Previous</span>
          </button>
          <button class="button-hv-reveal cursor-pointer flex-inline flex-flow-row align-center justify-center" v-on:click="this.setCurrentSectionNext()">
            <span>Next</span>
            <span class="margin-xs-left">&#10095;</span>
          </button>
        </div>
      </div>
      <hr class="margin-s-top margin-none-bottom" />
      <transition name="fade">
        <div v-show="isVisible.timelineSection === true" class="grid grid-col-1 grid-gap-xl align-center justify-center width-full height-full margin-auto-horizontal padding-m">
          <div class="vstack align-center margin-l-bottom">
            <div class="vstack align-center margin-s-bottom">
              <h3 class="margin-xs-top margin-none-bottom line-height-1 text-color-accent-primary font-scale-xl">{{ currentSection.position }}</h3>
              <h4 v-if="currentSection.company" class="margin-xs-top margin-none-bottom line-height-inherit">{{ currentSection.company }}</h4>
            </div>
            <span class="font-bold text-color-secondary">{{ currentSection.location }}</span>
            <span class="font-scale-s text-color-secondary">{{ currentSection.yearRange }}</span>
          </div>
          <div class="grid grid-gap-s" v-bind:class="isMobile === true ? 'grid-col-1' : 'grid-col-auto-fill-320 grid-auto-rows-1fr'">
            <div v-for="contentParagraph in currentSection.content" v-bind:key="contentParagraph[0] + contentParagraph.length" class="grid-item card padding-m padding-s-top padding-s-bottom" v-bind:class="isMobile === true ? '' : 'height-full'" v-html="this.formatMarkdown(contentParagraph)"></div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
import UserAgent from '@/utils/UserAgent.js';
import experienceJSON from '@/assets/experience.json';

export default {
  name: 'ProfessionalExperience',
  data() {
    return {
      isVisible: {
        tooltip: true,
        timelineSection: false
      },
      state: {
        currentSectionId: '',
        timelinePointHoverId: ''
      },
      tooltipValue: '',
      experience: []
    }
  },
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
    },
    currentSection: function () {
      return this.experience.find(x => x.id === this.state.currentSectionId);
    },
    currentIndex: function () {
      return this.experience.findIndex(x => x.id === this.state.currentSectionId);
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
      if (this.experience.find(x => x.id === this.state.currentSectionId)) {
        if (this.currentIndex <= 0) {
          this.setCurrentSection(this.experience[this.experience.length - 1].id);
        } else {
          this.setCurrentSection(this.experience[this.currentIndex - 1].id);
        }
      } else {
        console.log("Invalid index");
      }
    },
    setCurrentSectionNext: function () {
      if (this.experience.find(x => x.id === this.state.currentSectionId)) {
        if (this.currentIndex >= this.experience.length - 1) {
          this.setCurrentSection(this.experience[0].id);
        } else {
          this.setCurrentSection(this.experience[this.currentIndex + 1].id);
        }
      } else {
        console.log("Invalid index");
      }
    },
    showTooltip: function (sectionId) {
      if (this.isMobile === false) {
        const hoverSection = this.experience.find(x => x.id === sectionId);
        document.getElementById('proExpTooltip').innerHTML = hoverSection.company ? hoverSection.company : hoverSection.position;
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
      const tooltip = document.getElementById('proExpTooltip');
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
  beforeMount: function () {
    this.isVisible.timelineSection = true;
    this.experience = experienceJSON;
    this.state.currentSectionId = this.experience[0].id;
  }
}
</script>

<style scoped>
ul {
  margin: 0;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0;
  list-style-type: disc;
  list-style-position: inside;
}

.grid-item {
  align-self: flex-start;
}
</style>