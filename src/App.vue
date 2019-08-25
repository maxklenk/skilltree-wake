<template>
  <div id="app" class="bg-gray-200">

    <table>
      <!-- categories -->
      <tr>
        <td
          v-bind:class="colors[1][0]"
        >

        </td>
        <td
          v-for="(category, index) in categories"
          v-bind:class="colors[1][index % 2]"
        >
          <div class="category-label">
            {{ category }}
          </div>
        </td>
      </tr>

      <!-- levels -->
      <tr v-for="level in levels">
        <td
          v-bind:class="colors[level % 2][0]"
        >
          <div class="level-label">
            Level {{ level }}
          </div>
        </td>
        <td
          v-for="(category, index) in categories"
          v-bind:class="colors[level % 2][index % 2]"
          >
          <div class="flex justify-center">
            <Skill v-bind:checks="checks" v-bind:skill="skill" v-for="skill in skillsInCategoryAndLevel(category, level)"/>
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import Skill from './components/Skill.vue';
import skills from './skills';

export default {
  name: 'app',
  components: {
    Skill,
  },
  data() {
    return {
      checks: {
        theory: {
          icon: 'book',
          color: 'gray',
          level: 2,
          name: 'Theory',
        },
        regular: {
          icon: 'check',
          color: 'green',
          level: 2,
          name: 'Regular',
        },
        switch: {
          icon: 'rotate',
          color: 'purple',
          level: 2,
          name: 'Switch',
        },
      },
      categories: [
        'start',
        'surface',
        'ollie',
        // 'invert',
      ],
      levels: [
        1,
        2,
        3,
        4,
      ],
      colors: [
        [
          'bg-blue-200',
          'bg-blue-300',
        ],
        [
          'bg-indigo-300',
          'bg-indigo-200',
        ],
      ],
      skills: skills
    };
  },
  methods: {
    skillsInCategoryAndLevel(category, level) {
      return this.skills.filter(skill => {
        return skill.category === category && skill.level === level;
      });
    },
  }
};

// load youtube iframe_api
let tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
let firstScriptTag = document.getElementsByTagName('script')[0];
firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
</script>

<style lang="scss">
#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  min-height: 100%;
}
  .category-label {
    text-transform: uppercase;
    font-size: 40px;
    color: white;
  }
  .level-label {
    text-transform: uppercase;
    font-size: 40px;
    color: white;
    transform: rotate(270deg);
    white-space: nowrap;
  }
</style>
