<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header">
      <h2>Education</h2>
    </div>
    <div class="grid grid-col-1 grid-gap-m width-full width-max-800 margin-auto-horizontal">
      <div v-for="item in education" v-bind:key="item.id" class="grid-item vstack width-full padding-s-top padding-s-bottom">
        <div class="flex-inline flex-flow-row-wrap align-center justify-space-between width-full margin-xs-bottom font-scale-xs text-color-secondary text-transform-uppercase">
          <span class="font-bold">{{ item.level }}</span>
          <span>{{ item.location }}</span>
        </div>
        <div class="vstack margin-s-bottom">
          <h4 class="margin-xs-top margin-xs-bottom font-bold font-scale-l text-color-primary text-transform-none">{{ item.placeName }}</h4>
          <span v-if="item.degree" class="font-bold text-color-secondary">{{ item.degree }}</span>
          <div class="margin-xs-top">{{ item.dateRange }}</div>
        </div>
        <div v-if="item.awards" class="card width-full margin-xs-top margin-s-bottom padding-m padding-s-top padding-s-bottom">
          <h5 class="margin-xs-top margin-s-bottom font-scale-m text-color-secondary">Awards / Scholarships</h5>
          <ul>
            <li v-for="award in item.awards" v-bind:key="award">{{ award }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import educationJSON from '@/assets/education.json';

export default {
  name: 'Education',
  data() {
    return {
      education: educationJSON
    }
  }
}
</script>

<style scoped>
ul {
  margin: 0;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0;
  list-style-position: inside;
}

.grid-item {
  border-bottom: var(--CARD-BORDER-WIDTH) solid var(--CARD-BORDER-COLOR);
}
.grid-item:last-child {
  border-bottom: none;
}
</style>