<template>
  <Navbar />
  <div class="vstack vstack-height-full align-center justify-start width-full">
    <router-view />
    <Footer />
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Footer from '@/components/Footer.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Footer
  }
}
</script>

<style>
@import './assets/style/base.css';
@import './assets/style/main.css';
@import './assets/style/button.css';
@import './assets/style/form.css';
@import './assets/style/card.css';
@import './assets/style/fonts/fonts.css';
@import './assets/style/themes/dark.css' all and (prefers-color-scheme: dark);
@import './assets/style/themes/light.css' all and (prefers-color-scheme: light);
/* @import './assets/style/themes/light.css'; */

@import url('https://fonts.googleapis.com/css2?family=Commissioner:wght@300;400;700;800;900&family=Piazzolla:ital,wght@0,300;0,400;0,700;0,800;0,900;1,300;1,400;1,700;1,800;1,900&display=swap');
</style>
