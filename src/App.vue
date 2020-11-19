<template>
  <NavSidebar />
  <div class="vstack vstack-height-full width-full margin-auto-horizontal">
    <Hello id="top" />
    <AboutMe id="about" />
    <Skills id="skills" />
    <PersonalQualities id="qualities" />
    <Education id="edu" />
    <ProfessionalExperience id="experience" />
    <TechnicalJourney id="techjourney" />
    <Contact id="contact" />
    <Footer />
  </div>
</template>

<script>
import NavSidebar from '@/components/NavSidebar.vue';
import Hello from '@/components/Hello.vue';
import AboutMe from '@/components/AboutMe.vue';
import Skills from '@/components/Skills.vue';
import Education from '@/components/Education.vue';
import PersonalQualities from '@/components/PersonalQualities.vue';
import TechnicalJourney from '@/components/TechnicalJourney.vue';
import ProfessionalExperience from '@/components/ProfessionalExperience.vue';
import Contact from '@/components/Contact.vue';
import Footer from '@/components/Footer.vue';

export default {
  name: 'App',
  components: {
    NavSidebar,
    Hello,
    AboutMe,
    Skills,
    Education,
    PersonalQualities,
    TechnicalJourney,
    ProfessionalExperience,
    Contact,
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

.section {
  width: 100%;
  margin-bottom: 0 !important;
}
.section:last-child {
  margin-bottom: 5rem !important;
}
.section:nth-child(odd) {
  background: var(--BG-COLOR);
}
.section:nth-child(even) {
  background: var(--BG-SECONDARY-COLOR);
}
</style>
