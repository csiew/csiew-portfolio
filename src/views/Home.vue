<template>
  <div class="width-full margin-auto-horizontal">
    <Hello />
    <AboutMe />
    <Skills />
    <Education />
    <ProfessionalExperience />
    <TechnicalJourney />
    <Contact />
  </div>
</template>

<script>
import Hello from '@/components/Hello.vue';
import AboutMe from '@/components/AboutMe.vue';
import Skills from '@/components/Skills.vue';
import Education from '@/components/Education.vue';
import TechnicalJourney from '@/components/TechnicalJourney.vue';
import ProfessionalExperience from '@/components/ProfessionalExperience.vue';
import Contact from '@/components/Contact.vue';

export default {
  name: 'Home',
  components: {
    Hello,
    AboutMe,
    Skills,
    Education,
    TechnicalJourney,
    ProfessionalExperience,
    Contact
  }
}
</script>

<style scoped>
.section {
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