<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-bottom">
      <h2>Projects</h2>
    </div>
    <div class="grid grid-gap-xl width-full width-max-1024 margin-auto-horizontal margin-xl-bottom" v-bind:class="isMobile === true ? 'grid-col-1' : 'grid-col-1-3'">
      <div v-if="isMobile === false" class="grid-item nav-border-right vstack height-full margin-none padding-m-right text-align-right">
        <div v-for="item in projects" v-bind:key="item.id" class="tabbar-vertical-item cursor-pointer" v-bind:class="state.currentProjectId === item.id ? 'tabbar-vertical-item-selected' : ''" v-on:click="setSelectedProject(item.id)">{{ item.name }}</div>
      </div>
      <div v-else class="grid-item nav-border-bottom hstack overflow-x-auto align-center justify-stretch width-full padding-xs-bottom">
        <div v-for="item in projects" v-bind:key="item.id" class="project-tab tabbar-vertical-item margin-none padding-xs-top padding-xs-bottom padding-s-left padding-s-right text-align-center" v-bind:class="state.currentProjectId === item.id ? 'tabbar-vertical-item-selected' : ''" v-on:click="setSelectedProject(item.id)">{{ item.name }}</div>
      </div>
      <transition name="fade">
        <div v-show="isVisible.projectCard === true" class="grid-item vstack card width-full padding-m">
          <div class="margin-s-bottom text-align-center">
            <h3 class="margin-xs-top margin-none-bottom line-height-1 text-color-accent-primary font-scale-xxl">{{ selectedProject.name }}</h3>
            <h4 class="margin-xs-top margin-none-bottom line-height-inherit">{{ selectedProject.timeRange }}</h4>
          </div>
          <hr class="margin-xs-top margin-xs-bottom" />
          <div v-if="selectedProject.imgUrl" class="margin-s-top margin-s-bottom">
            <a v-bind:href="getProjectImgUrl(selectedProject.imgUrl)"><img v-bind:alt="selectedProject.imgUrl" class="card card-img" v-bind:src="getProjectImgUrl(selectedProject.imgUrl)" style="width: 100%; height: auto;" /></a>
          </div>
          <div class="margin-s-bottom" v-html="formatMarkdown(selectedProject.description)"></div>
          <div class="grid grid-col-2 grid-gap-s width-full">
            <a class="button" v-bind:class="!selectedProject.url ? 'button-disabled' : ''" v-bind:href="selectedProject.url ? selectedProject.url : null" target="_blank" v-bind:title="selectedProject.url ? 'Visit project website' : 'Project does not have a website'">Website</a>
            <a class="button" v-bind:class="!selectedProject.github ? 'button-disabled' : ''" v-bind:href="selectedProject.github ? selectedProject.github : null" target="_blank" v-bind:title="selectedProject.url ? 'Visit project repository' : 'Project does not have a repository'">GitHub</a>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
import UserAgent from '@/utils/UserAgent.js';
import projectsJSON from '@/assets/projects.json';

export default {
  name: 'Projects',
  data() {
    return {
      isVisible: {
        projectCard: true
      },
      state: {
        currentProjectId: '',
      },
      projects: []
    }
  },
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
    },
    selectedProject: function () {
      return this.projects.find(x => x.id === this.state.currentProjectId);
    }
  },
  methods: {
    formatMarkdown: function (content) {
      return marked(content, { sanitized: true });
    },
    getProjectImgUrl: function (imageUrl) {
      try {
        return require(`@/assets/images/projects/${imageUrl}`);
      } catch (err) {
        console.warn(err);
        return;
      }
    },
    setSelectedProject: function (sectionId) {
      if (this.state.currentProjectId !== sectionId) {
        this.isVisible.projectCard = false;
        setTimeout(() => {
          this.state.currentProjectId = sectionId;
          this.isVisible.projectCard = true;
        }, 500);
      }
    }
  },
  beforeMount: function () {
    this.projects = projectsJSON;
    this.state.currentProjectId = this.projects[0].id;
  }
}
</script>

<style scoped>
.grid-item {
  align-self: flex-start;
}
.project-tab {
  width: auto;
  white-space: nowrap;
}
</style>