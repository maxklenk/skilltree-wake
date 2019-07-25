<template>
  <div id="app" class="bg-gray-200">
    <Skill v-bind:checks="checks" v-bind:skill="skill" v-for="skill in skills"/>
  </div>
</template>

<script>
import Skill from './components/Skill.vue';

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
      skills: [
        {
          state: {
            checks: {
              theory: 0,
              regular: 0,
              switch: 0,
            },
          },
          attributes: {
            rotate: false,
            board: 'HS',
            jump: true,
            degree: 0,
          },
          name: 'Ollie',
          requirements: [],
          resources: [
            {
              type: 'video',
              platform: 'youtube',
              id: 'csBs5at14nA',
              start: 20,
              title: 'Ollie Wakeboard Tutorial [GER]',
              lang: 'de',
            },
          ],
        },
        {
          state: {
            checks: {
              theory: 0,
              regular: 0,
              switch: 0,
            },
          },
          attributes: {
            rotate: 'FS',
            board: 'HS',
            jump: true,
            degree: 180,
          },
          name: 'Ollie HS FS 180',
          requirements: [],
          resources: [
            {
              type: 'video',
              platform: 'youtube',
              id: 'FUp1gZrz2rE',
              start: 20,
              title: 'Ollie HS FS 180 Wakeboard Tutorial [GER]',
              lang: 'de',
            },
          ],
        },
        {
          state: {
            checks: {
              theory: 0,
              regular: 0,
              switch: 0,
            },
          },
          attributes: {
            rotate: 'BS',
            board: 'HS',
            jump: true,
            degree: 180,
          },
          name: 'Ollie HS BS 180',
          requirements: [],
          resources: [
            {
              type: 'video',
              platform: 'youtube',
              id: '4Gvq6yTnHHc',
              start: 20,
              title: 'Ollie HS BS 180 Wakeboard Tutorial [GER]',
              lang: 'de',
            },
          ],
        },
      ]
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  min-height: 100%;
}
</style>
