<template>
  <div class="section width-full text-align-center margin-auto-horizontal margin-xxl-bottom padding-xl padding-xxl-top padding-xxl-bottom">
    <div class="section-header margin-xxl-top padding-xxl-bottom">
      <img class="profile-img profile-img-xl nodrag noselect border-radius-100pct" alt="profile" v-bind:src="useAltProfile === false ? require('@/assets/images/profile_0.jpg') : require('@/assets/images/profile_1.jpg')" v-on:click="useAltProfile = !useAltProfile" />
      <h1 class="flex-inline width-full justify-center align-center margin-xl-top margin-xl-bottom text-align-center">Clarence Siew</h1>
      <button class="button-hv-reveal padding-l padding-m-top padding-m-bottom font-scale-xxl" v-on:click="this.scrollToAbout()">&darr;</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hello',
  data() {
    return {
      useAltProfile: false
    }
  },
  methods: {
    scrollToAbout: function () {
      try {
        const el = document.getElementById('about');
        el && el.scrollIntoView({
          block: 'start',
          behavior: 'smooth'
        });
      } catch (err) {
        console.warn(err);
      }
    }
  }
}
</script>

<style scoped>
h1, h2, h3 {
  letter-spacing: -0.0625rem !important;
}
h1 {
  color: var(--FG-COLOR);
  font-size: 12vmin !important;
}
h2 {
  color: var(--FG-SECONDARY-COLOR);
  font-size: 9vmin !important;
}
h3 {
  color: var(--FG-SECONDARY-COLOR);
  font-size: 6vmin !important;
}
</style>