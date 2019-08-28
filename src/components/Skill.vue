<template>
  <div class="skill-place">
  <div
    class="skill relative p-8 inline-block"
    v-bind:class="{'is-active': state.selected}"
    v-bind:style="skillStyling"
  >
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
      class="close rounded-full shadow flex justify-center items-center absolute border-4 cursor-pointer"
      v-on:click="select()"
    >
      X
    </div>

    <div
      class="rounded-full overflow-hidden shadow-lg h-full w-full bg-white border-8"
      v-bind:class="skillClasses()"
      v-on:click="select()"
    >
      <div class="h-full relative">
        <div class="h-full w-full videoBox"></div>
        <img
          class="h-full w-full"
          v-bind:src="'https://img.youtube.com/vi/' + skill.resources[0].id + '/hqdefault.jpg'"
        >

<!--        16:9 -->
<!--        <iframe-->
<!--          class="h-full w-full"-->
<!--          width="400"-->
<!--          height="225"-->
<!--          v-bind:src="'https://www.youtube.com/embed/' + skill.resources[0].id + '?start=' + skill.resources[0].start"-->
<!--          frameborder="0"-->
<!--          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"-->
<!--          allowfullscreen-->
<!--        ></iframe>-->
        <div class="skill-name">
          <h2 class="text-xl leading-tight">{{ skill.name }}</h2>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'Skill',
  data() {
    return {
      skillStyling: {},
      state: {
        selected: false,
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
    progress: Object,
    saveProgress: Function,
  },
  methods: {
    select() {
      let elment = this.$el;
      if (!this.state.selected) {
        // styling
        this.skillStyling = {
          transform: 'translate3d(' + -(elment.children[0].offsetLeft - window.scrollX) + 'px, ' + -(elment.children[0].offsetTop - window.scrollY) + 'px, 0)',
        };
        this.state.selected = true;

        // start video
        if (!this.player) {
          this.player = new window.YT.Player(elment.getElementsByClassName('videoBox')[0], {
            height: '390',
            width: '640',
            videoId: this.skill.resources[0].id,
            events: {
              'onReady': function(event) {
                event.target.playVideo();
              },
              'onStateChange': function(event) {
                if (event.data === window.YT.PlayerState.PLAYING) {
                  elment.getElementsByClassName('videoBox')[0].classList.add('is-playing');
                }
              }
            }
          });
        } else {
          this.player.playVideo();
          elment.getElementsByClassName('videoBox')[0].classList.add('is-playing');
        }
      } else {
        this.skillStyling = {};
        this.state.selected = false;
        elment.getElementsByClassName('videoBox')[0].classList.remove('is-playing');
        this.player.pauseVideo();
      }
    },
    getColorWeight(level) {
      return level * 300 + 100;
    },
    checkClasses(check, name, index) {
      return [
        `check-${index}`,
        this.progress[this.skill.id] ? `border-${check.color}-${this.getColorWeight(this.progress[this.skill.id][name])}`: ``,
        `bg-${check.color}-100`,
        `hover:text-white`,
        `hover:bg-${check.color}-500`,
      ];
    },
    checkUpdate(check, name) {
      this.progress[this.skill.id][name] = (this.progress[this.skill.id][name] + 1) % (this.checks[name].level + 1);
      this.saveProgress(this.skill.id, this.progress[this.skill.id]);
    },
    skillClasses() {
      if (!this.progress[this.skill.id]) {
        return [];
      }

      let highest = null;
      for (const check in this.progress[this.skill.id]) {
        if (highest == null) {
          highest = check;
        }
        if (this.progress[this.skill.id][check]) {
          highest = check;
        }
      }

      return [
        `border-${this.checks[highest].color}-${this.getColorWeight(this.progress[this.skill.id][highest])}`,
      ];
    },
  },
};
</script>

<style lang="scss">
  .videoBox {
    position: absolute;
    opacity: 0;

    &.is-playing {
      opacity: 1;
    }
  }
</style>
<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .skill-place {
    width: 480px;
    height: 480px;
  }

  .skill {
    width: 480px;
    height: 480px;
    transition: transform 400ms;
    transform: translate3d(0, 0, 0);

    &.is-active {
      width: 100vw;
      height: 100vh;
      z-index: 10;

      .rounded-full {
        border-radius: 30px;
      }
      .close {
        display: flex;
        z-index: 20;
      }
      .check {
        z-index: 20;
      }
      .check-0 {
        top: 0;
        right: 69px;
      }
      .check-1 {
        top: 30px;
      }
      .check-2 {
        top: 77px;
      }
      .skill-name {
        display: none;
      }
    }
  }
  .close {
    display: none;
    width: 64px;
    height: 64px;
    background: white;
    top: 10px;
    left: 10px;

    &:hover {
      border-color: white;
      background: gray;
      color: white;
    }
  }
  .check {
    width: 64px;
    height: 64px;
    z-index: 5;
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
  .skill-name {
    position: absolute;
    bottom: 0;
    padding: 20px;
    background: white;
    width: 100%;
  }
</style>
