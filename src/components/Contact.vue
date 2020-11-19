<template>
  <div class="section margin-auto-horizontal padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header">
      <h2>Contact</h2>
    </div>
    <div class="grid grid-col-3 grid-col-auto-fit-640 grid-gap-m width-full width-max-800 margin-auto-horizontal">
      <span v-for="item in links" v-bind:key="item.id">
        <a class="grid-item button width-full" v-bind:href=item.url target="_blank">{{ item.name }}</a>
      </span>
    </div>
  </div>
</template>

<script>
import linksJSON from '@/assets/links.json';

export default {
  name: 'Contact',
  data() {
    return {
      links: linksJSON.links
    }
  }
}
</script>