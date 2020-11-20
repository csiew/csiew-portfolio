<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-bottom">
      <h2>Technical Journey</h2>
    </div>
    <div class="grid grid-col-1 grid-gap-l width-full width-max-1024 margin-auto-horizontal">
      <div id="techJourneyTooltip" v-show="isVisible.tooltip === true" class="tooltip"></div>
      <div class="vstack width-full align-center justify-center margin-auto-horizontal padding-l-top padding-m-left padding-m-right">
        <div class="timeline width-full margin-l-left margin-l-right" v-on:mousemove="this.setTooltipPosition" v-on:mouseout="this.hideTooltip()">
          <button v-for="item in this.techjourney" v-bind:key="item.id" class="timeline-point" v-bind:class="(item.id === state.currentSectionId) ? 'button-selected timeline-point-selected' : ''" v-on:mouseover="this.showTooltip(item.id)" v-on:mouseout="this.hideTooltip()" v-on:click="this.setCurrentSection(item.id)"></button>
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
        <div v-show="isVisible.timelineSection === true" class="grid grid-gap-xl align-center justify-center width-full margin-auto-horizontal padding-m" v-bind:class="currentSection.image ? 'grid-col-auto-fill-320' : 'grid-col-1'">
          <div v-if="currentSection.image && currentSection.image.align === 'left'" class="grid-item vstack align-center justify-center">
            <img v-bind:src="getImage(currentSection.image.src)" class="grid-item card card-img width-full aligh-center justify-center noselect nodrag" v-bind:alt="currentSection.image.alt" />
            <div v-if="currentSection.image.caption" class="caption width-full padding-s">{{ currentSection.image.caption }}</div>
          </div>
          <div class="grid-item flex-inline flex-flow-column aligh-center justify-center">
            <h3 class="margin-xs-top margin-xs-bottom line-height-inherit font-scale-xl">{{ currentSection.title }}</h3>
            <div v-for="contentParagraph in currentSection.content" v-bind:key="contentParagraph[0] + contentParagraph.length" v-html="this.formatMarkdown(contentParagraph)"></div>
          </div>
          <div v-if="currentSection.image && currentSection.image.align === 'right'" class="grid-item vstack align-center justify-center">
            <img v-bind:src="getImage(currentSection.image.src)" class="grid-item card card-img width-full aligh-center justify-center noselect nodrag" v-bind:alt="currentSection.image.alt" />
            <div v-if="currentSection.image.caption" class="caption width-full margin-xs-top padding-s">{{ currentSection.image.caption }}</div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
import UserAgent from '@/utils/UserAgent.js';
import techjourneyJSON from '@/assets/techjourney.json';

export default {
  name: 'TechnicalJourney',
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
      techjourney: []
    }
  },
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
    },
    currentSection: function () {
      return this.techjourney.find(x => x.id === this.state.currentSectionId);
    },
    currentIndex: function () {
      return this.techjourney.findIndex(x => x.id === this.state.currentSectionId);
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
      if (this.techjourney.find(x => x.id === this.state.currentSectionId)) {
        if (this.currentIndex <= 0) {
          this.setCurrentSection(this.techjourney[this.techjourney.length - 1].id);
        } else {
          this.setCurrentSection(this.techjourney[this.currentIndex - 1].id);
        }
      } else {
        console.log("Invalid index");
      }
    },
    setCurrentSectionNext: function () {
      if (this.techjourney.find(x => x.id === this.state.currentSectionId)) {
        if (this.currentIndex >= this.techjourney.length - 1) {
          this.setCurrentSection(this.techjourney[0].id);
        } else {
          this.setCurrentSection(this.techjourney[this.currentIndex + 1].id);
        }
      } else {
        console.log("Invalid index");
      }
    },
    showTooltip: function (sectionId) {
      if (this.isMobile === false) {
        const hoverSection = this.techjourney.find(x => x.id === sectionId);
        document.getElementById('techJourneyTooltip').innerHTML = hoverSection.title;
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
  beforeMount: function () {
    this.isVisible.timelineSection = true;
    this.techjourney = techjourneyJSON;
    this.state.currentSectionId = this.techjourney[0].id;
  }
}
</script>

<style scoped>
.grid-item {
  align-self: flex-start;
}
</style>