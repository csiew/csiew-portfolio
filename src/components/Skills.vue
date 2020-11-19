<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header">
      <h2>Skills</h2>
    </div>
    <div class="width-full width-max-800 margin-auto-horizontal">
      <div class="hstack margin-l-bottom">
        <input class="width-full" type="text" placeholder="Filter..." @keyup="filter" v-model="filterTerm" />
        <button v-if="this.filterTerm.length > 0" class="margin-s-left cursor-pointer" v-on:click="this.resetResults()">Clear</button>
      </div>
      <span v-if="this.filterTerm.length > 0">
        <h3>Results ({{ result.length }} items)</h3>
        <div class="grid grid-gap-m grid-col-1 grid-col-auto-fit-640">
          <div class="grid-item list-freestyle">
            <ul>
              <li v-for="item in this.result" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
        </div>
      </span>
      <span v-else>
        <h3>Programming Languages</h3>
        <div class="grid grid-gap-m grid-col-1 grid-col-auto-fit-640">
          <div class="grid-item list-freestyle">
            <ul>
              <li v-for="item in this.skills['programmingLangs']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
        </div>
        <hr />
        <h3>Frameworks</h3>
        <div class="grid grid-gap-m grid-col-2 grid-col-auto-fit-640">
          <div class="grid-item list-freestyle">
            <h4>Frontend</h4>
            <ul>
              <li v-for="item in this.skills['frameworks']['frontend']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
          <div class="grid-item list-freestyle">
            <h4>Backend</h4>
            <ul>
              <li v-for="item in this.skills['frameworks']['backend']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
        </div>
        <hr />
        <h3>Infrastructure</h3>
        <div class="grid grid-gap-m grid-col-2 grid-col-auto-fit-640">
          <div class="grid-item list-freestyle">
            <h4>Database</h4>
            <ul>
              <li v-for="item in this.skills['infrastructure']['database']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
          <div class="grid-item list-freestyle">
            <h4>Cloud</h4>
            <ul>
              <li v-for="item in this.skills['infrastructure']['cloud']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
          <div class="grid-item list-freestyle">
            <h4>Continuous Integration</h4>
            <ul>
              <li v-for="item in this.skills['infrastructure']['ci']" v-bind:key="item">{{ item }}</li>
            </ul>
          </div>
        </div>
      </span>
    </div>
  </div>
</template>

<script>
import skillsJSON from '@/assets/skills.json';

export default {
  name: 'Skills',
  data() {
    return {
      skills: skillsJSON,
      filterTerm: '',
      result: []
    }
  },
  methods: {
    filter: function () {
      this.result = this.skills['programmingLangs'].concat(this.skills['frameworks']['frontend'], this.skills['frameworks']['backend'], this.skills['infrastructure']['database'], this.skills['infrastructure']['cloud'], this.skills['infrastructure']['ci']).filter(item => item.toLowerCase().includes(this.filterTerm.toLowerCase()));
    },
    resetResults: function () {
      this.filterTerm = "";
      this.result = [];
    }
  }
}
</script>

<style scoped>
h3, h4 {
  margin-top: 0.25rem;
}
h3 {
  margin-bottom: 1.25rem;
}
h4 {
  margin-bottom: 1rem;
}
</style>