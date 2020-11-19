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

export default {
  name: 'PersonalQualities',
  data() {
    return {
      qualities: [
        {
          id: 'communicator',
          title: 'Clear & Inclusive Communicator',
          content: "I strive to be a *clear* and *expressive* communicator. I also strive to be an *inclusive* communicator that does not leave anyone behind; bringing everyone together to assemble as broad and large a picture as possible."
        },
        {
          id: 'learner',
          title: 'Intrepid Learner',
          content: "Learning new and different things is one of my core qualities. Not only do I quickly pickup new skills, but I am always on the lookout for even *more* things to learn."
        },
        {
          id: 'problemsolver',
          title: 'Pragmatic Problem Solver',
          content: "I love to solve problems in just about every aspect of my life. But I believe that problem solving goes even further when one is pragmatic. Sometimes the best solutions are those that we too often overlook, and it often takes a pragmatist to adhere to the rule of [Ockham's Razor](https://en.wikipedia.org/wiki/Occam's_razor)."
        },
        {
          id: 'compassionate',
          title: 'Compassionate',
          content: "I believe that being one's most compassionate self helps to bring out the best in myself and those around me. Building a consensus or bridging gaps in points of view is perhaps the ultimate amalgamation of communicating, learning, problem solving, and being a team player as it helps to build a team environment that is *trusting* and *open* to different ideas or points of view."
        }
      ]
    }
  },
  methods: {
    convertMarkdown: function (content) {
      return marked(content, { sanitized: true });
    }
  }
}
</script>