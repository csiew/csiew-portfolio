<template>
  <div class="section width-full text-align-center margin-auto-horizontal margin-xl-bottom padding-l nodrag">
    <div id="helloContent" class="section-header hstack flex-flow-row-wrap align-center justify-center margin-xxl-top padding-xxl-bottom">
      <img class="profile-img profile-img-l margin-l nodrag noselect border-radius-100pct" alt="profile" v-bind:src="useAltProfile === false ? require('@/assets/images/profile_0.jpg') : require('@/assets/images/profile_1.jpg')" v-on:click="useAltProfile = !useAltProfile" />
      <h1 class="flex-inline width-auto justify-center align-center text-align-center">Clarence Siew</h1>
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
  }
}
</script>

<style scoped>
h1, h2, h3 {
  letter-spacing: -0.25rem !important;
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