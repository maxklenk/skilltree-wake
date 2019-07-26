<template>
  <div class="skill relative p-8 inline-block">
    <div
      v-bind:key="index"
      v-for="(check, name, index) in checks"
      class="check rounded-full shadow flex justify-center items-center absolute border-4 cursor-pointer"
      v-bind:class="checkClasses(check, name, index)"
      v-on:click="checkUpdate(check, name, index)"
    >
      {{ check.name }}
    </div>

    <div
      class="rounded-full overflow-hidden shadow-lg h-full w-full bg-white border-8"
      v-bind:class="skillClasses()"
    >
      <div class="h-57p">
        <iframe
          class="h-full w-full"
          width="400"
          height="225"
          v-bind:src="'https://www.youtube.com/embed/' + skill.resources[0].id + '?start=' + skill.resources[0].start"
          frameborder="0"
          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </div>
      <div class="flex justify-center p-4">
        <h2 class="text-xl leading-tight">{{ skill.name }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Skill',
  data() {
    return {
      state: {
        checks: {
          theory: 0,
          regular: 0,
          switch: 0,
        },
      },
    };
  },
  props: {
    checks: Object,
    skill: Object,
  },
  methods: {
    getColorWeight(level) {
      return level * 300 + 100;
    },
    checkClasses(check, name, index) {
      return [
        `check-${index}`,
        `border-${check.color}-${this.getColorWeight(this.state.checks[name])}`,
        `bg-${check.color}-100`,
      ];
    },
    checkUpdate(check, name, index) {
      this.state.checks[name] = (this.state.checks[name] + 1) % (this.checks[name].level + 1);
    },
    skillClasses() {
      let highest = null;
      for (const check in this.state.checks) {
        if (highest == null) {
          highest = check;
        }
        if (this.state.checks[check]) {
          highest = check;
        }
      }

      return [
        `border-${this.checks[highest].color}-${this.getColorWeight(this.state.checks[highest])}`,
      ];
    },
  },
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .skill {
    width: 480px;
    height: 480px;
  }
  .check {
    width: 64px;
    height: 64px;
  }
  .h-57p {
    height: 57%;
  }
  .check-0 {
    top: 34px;
    right: 62px;
  }
  .check-1 {
    top: 70px;
    right: 25px;
  }
  .check-2 {
    top: 117px;
    right: 0;
  }
</style>
