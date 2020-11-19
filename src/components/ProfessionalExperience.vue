<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-bottom">
      <h2>Professional Experience</h2>
    </div>
    <div class="grid grid-col-1 grid-gap-l width-full width-max-1024 margin-auto-horizontal">
      <div id="proExpTooltip" v-show="isVisible.tooltip === true" class="tooltip"></div>
      <div class="vstack width-full align-center justify-center margin-auto-horizontal padding-l-top padding-m-left padding-m-right">
        <div class="timeline width-full margin-l-left margin-l-right" v-on:mousemove="this.setTooltipPosition" v-on:mouseout="this.hideTooltip()">
          <button v-for="pointId in Object.keys(this.sections)" v-bind:key="`timeline-point-${pointId}`" class="timeline-point" v-bind:class="(Number(pointId) === state.currentSectionId) ? 'button-selected timeline-point-selected' : ''" v-on:mouseover="this.showTooltip(Number(pointId))" v-on:mouseout="this.hideTooltip()" v-on:click="this.setCurrentSection(Number(pointId))"></button>
        </div>
        <div class="hstack hstack-space-between align-center margin-xl-top">
          <button class="button-borderless cursor-pointer flex-inline flex-flow-row align-center justify-center padding-none line-height-1 font-scale-l" v-on:click="this.setCurrentSectionPrevious()">
            <span>&#10229;</span>
            <span class="padding-xs">Previous</span>
          </button>
          <button class="button-borderless cursor-pointer flex-inline flex-flow-row align-center justify-center padding-none line-height-1 font-scale-l" v-on:click="this.setCurrentSectionNext()">
            <span class="padding-xs">Next</span>
            <span>&#10230;</span>
          </button>
        </div>
      </div>
      <hr class="margin-l-top margin-none-bottom" />
      <transition name="fade">
        <div v-show="isVisible.timelineSection === true" class="grid grid-col-1 grid-gap-xl align-center justify-center width-full margin-auto-horizontal padding-m">
          <div class="grid-item flex-inline flex-flow-column aligh-center justify-center">
            <div class="vstack margin-m-bottom">
              <h3 class="margin-xs-top margin-s-bottom line-height-inherit text-color-secondary"><span class="text-color-accent-primary">{{ this.sections[state.currentSectionId].position }}</span><span v-if="this.sections[state.currentSectionId].company"> at <span class="text-color-accent-primary">{{ this.sections[state.currentSectionId].company }}</span></span></h3>
              <span class="font-bold text-color-secondary">{{ this.sections[state.currentSectionId].location }}</span>
              <span class="font-scale-s text-color-secondary">{{ this.sections[state.currentSectionId].yearRange }}</span>
            </div>
            <div class="grid grid-col-auto-fill-320 grid-auto-rows-1fr grid-gap-s">
              <div v-for="contentParagraph in this.sections[state.currentSectionId].content" v-bind:key="contentParagraph[0] + contentParagraph.length" class="grid-item card padding-m padding-s-top padding-s-bottom" v-bind:class="isMobile === true ? '' : 'height-full'" v-html="this.formatMarkdown(contentParagraph)"></div>
            </div>
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
  name: 'ProfessionalExperience',
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
          position: 'Student Intern',
          company: 'Trend Labs (Trend Micro Philippines)',
          location: 'Manila, Philippines',
          yearRange: '2013',
          content: [
            'Developed a mobile game to celebrate the launch of Trend Micro\'s then-latest antivirus product.',
            'Engaged with quality assurance engineers to test and debug pre-release software.',
            'Reviewed and performed corrections to technical documentation.'
          ]
        },
        1: {
          position: 'IBL Student Intern',
          company: 'National Australia Bank',
          location: 'Melbourne, VIC, Australia',
          yearRange: 'July 2018 - December 2018',
          content: [
            'Reviewed and uplifted legacy banking services by designing and implementing serverless microservices for internal banking processes using the Spring Framework and Amazon Web Services (AWS) products.',
            'Developed dashboards for the monitoring of serverless microservices on AWS.',
            'Uplifted internal quality assurance tools with a graphing library to represent calculation processes for debugging and validation.',
            'Performed quality assurance by formulating use cases and implementing test cases.',
            'Participated and facilitated agile ceremonies.',
            'Standardised and wrote documentation for Git version control procedures for team workflow.'
          ]
        },
        2: {
          position: 'Consultant',
          location: 'Melbourne, VIC, Australia',
          yearRange: 'August 2020 - September 2020',
          content: [
            'Reviewed and assessed an existing proof-of-concept project for a healthcare-related non-government organisation.',
            'Reported issues relating to software architecture, user experience, and project management',
            'Outlined potential pathways to realign project objectives with client\'s vision.'
          ]
        }
      }
    }
  },
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
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
      if (this.isMobile === false) {
        document.getElementById('proExpTooltip').innerHTML = this.sections[sectionId].company ? this.sections[sectionId].company : this.sections[sectionId].position;
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
  mounted: function () {
    this.setCurrentSection(0);
    this.isVisible.timelineSection = true;
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