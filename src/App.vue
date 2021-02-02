<template>
  <NavSidebar v-bind:helloIsVisible="isVisible.hello" />
  <div class="vstack vstack-height-full width-full margin-auto-horizontal">
    <Hello id="top" />
    <AboutMe id="about" />
    <Skills id="skills" />
    <PersonalQualities id="qualities" />
    <Education id="edu" />
    <ProfessionalExperience id="experience" />
    <TechnicalJourney id="techjourney" />
    <Projects id="projects" />
    <PersonalWebsite id="website" />
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
import Projects from '@/components/Projects.vue';
import PersonalWebsite from '@/components/PersonalWebsite.vue';
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
    Projects,
    PersonalWebsite,
    Contact,
    Footer
  },
  data() {
    return {
      isVisible: {
        hello: true
      }
    }
  },
  methods: {
    helloIsVisible: function () {
      // Adapted from https://gist.github.com/jjmu15/8646226
      var element = document.getElementById("helloContent");
      var rect = element.getBoundingClientRect();
      this.isVisible.hello = (rect.bottom > 0);
    },
  },
  mounted: function () {
    document.addEventListener('scroll', this.helloIsVisible);
  }
}
</script>

<style>
@import './assets/style/base.css';
@import './assets/style/main.css';
@import './assets/style/button.css';
@import './assets/style/form.css';
@import './assets/style/card.css';
@import './assets/style/themes/light.css';
@import './assets/style/themes/dark.css';
@import './assets/style/fonts/fonts.css';

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
