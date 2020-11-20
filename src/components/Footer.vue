<template>
  <div class="footer anchor-bottom padding-xl margin-auto-top" v-bind:class="isMobile === true ? 'margin-xxl-bottom' : ''">
    <p class="text-color-secondary">
      &copy; 2020 Clarence Siew
    </p>
  </div>
</template>

<script>
import UserAgent from '@/utils/UserAgent.js';

export default {
  name: 'Footer',
  computed: {
    isMobile: function () {
      return UserAgent.isMobile();
    }
  }
}
</script>