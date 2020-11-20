<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header">
      <h2>Personal Qualities</h2>
    </div>
    <div class="grid grid-col-auto-fit-480 grid-gap-m width-full width-max-1024 margin-auto-horizontal margin-xl-bottom">
      <div v-for="item in qualities" v-bind:key="item.id" class="grid-item card padding-m">
        <h3>{{ item.title }}</h3>
        <div class="width-full" v-html="this.convertMarkdown(item.content)"></div>
      </div>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
import qualitiesJSON from '@/assets/qualities.json';

export default {
  name: 'PersonalQualities',
  data() {
    return {
      qualities: []
    }
  },
  methods: {
    convertMarkdown: function (content) {
      return marked(content, { sanitized: true });
    }
  },
  beforeMount: function () {
    this.qualities = qualitiesJSON;
  }
}
</script>