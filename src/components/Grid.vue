<template>
  <div class="grid">
    <div class="flex-wrap">
      <div
        class="container"
        v-for="(value, index) in gridData"
        :key="index"
      >
        <div
          class="container__box"
          :class="{ container__box__blink: value === 1 || value === 3 }"
        />
        <div
          :id="`ball${index}`"
          v-if="value === 2 || value === 3"
          class="container__ball"
        >
          0
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    gridData: {
      type: Array,
      default: () => [],
    },
    x: {
      type: Number,
      default: null,
    },
    y: {
      type: Number,
      default: null,
    },
    startMoving: {
      type: Boolean,
      default: false,
    },
    basePosition: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      keyframes: [],
      animationProperties: {
        duration: 500,
        iterations: Infinity,
        easing: "ease-out",
      },
    };
  },
  watch: {
    startMoving(newValue) {
      if (newValue) {
        this.setNewDestination();
      }
    },
  },
  methods: {
    setNewDestination() {
      for (let index = 0; index < 9; index++) {
        var element = document.getElementById(`ball${index}`);
        if (element && this.x && this.y && this.startMoving) {
          console.log("this.x", this.x);
          console.log("this.y", this.y);
          var translateY = this.y - this.basePosition[index].y;
          var translateX = this.x - this.basePosition[index].x;
          this.keyframes[index] = [
            { transform: "translate(-50%, -50%)" },
            { transform: `translate(${translateX}px, ${translateY}px)` },
          ];
        }
      }
    },
    moveBalls() {
      for (let index = 0; index < 9; index++) {
        var element = document.getElementById(`ball${index}`);
        if (element) {
          element.animate(this.keyframes[index], this.animationProperties);
        }
      }
    },
    init() {
      for (let index = 0; index < 9; index++) {
        var element = document.getElementById(`ball${index}`);
        if (element) {
          var rect = element.getBoundingClientRect();
          this.basePosition[index] = { x: rect.left, y: rect.top };
          this.keyframes[index] = [
            { transform: "translate(-50%, -50%)" },
            { transform: "translate(250px, -50%)" },
          ];
        }
      }
    },
  },
  mounted() {
    this.init();
    setInterval(() => {
      this.moveBalls();
    }, 1000);
  },
};
</script>

<style lang="scss" scoped>
@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
  100% {
    opacity: 1;
  }
}
@keyframes moveball {
  0% {
    transform: translate(0, -50%);
    // left: 50%;
    // top: 50%;
  }
  100% {
    transform: translate(250px, -50%);
    // left: 300%;
    // top: 50%;
  }
}
.grid {
  // background-color: red;
  overflow: hidden;
  height: 100vh;
  display: flex;
  align-items: center;
}
.flex-wrap {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  min-width: 100%;
  column-gap: 10px;
  .container {
    width: calc(33.33% - 24px);
    position: relative;

    &__box {
      height: 100px;
      margin: 10px 0 10px 0;
      border: black solid 2px;
      background: radial-gradient(
        circle,
        rgba(113, 81, 95, 1) 81%,
        rgba(0, 0, 0, 1) 100%
      );
      &__blink {
        animation: blink 1s infinite;
      }
    }
    &__ball {
      left: 50%;
      top: 50%;
      width: 30px;
      height: 30px;
      background-color: #a5f12b;
      border-radius: 50%;
      position: absolute;
      transform: translate(-50%, -50%);
      display: flex;
      align-items: center;
      justify-content: center;
      transition: left 0.5s;
      z-index: 2;
      // transition: transform 0.5s;
      // animation: moveball 1s infinite;
    }
  }
}
</style>
