<template>
  <div class="nine-square-grid">
    <!-- flex-wrap -->
    <div class="nine-square-grid__flex-wrap">
      <!-- 定位球用 -->
      <div
        class="nine-square-grid__flex-wrap__container"
        v-for="(value, index) in gridData"
        :key="index"
      >
        <!-- 格子背景，跟球同層級因此不會閃爍到球-->
        <div
          class="nine-square-grid__flex-wrap__container__grid-cell"
          :class="{
            'nine-square-grid__flex-wrap__container__grid-cell-blink':
              value === 1 || value === 3,
          }"
        />
        <!-- 球 -->
        <div
          :id="`ball${index}`"
          v-if="value === 2 || value === 3"
          class="nine-square-grid__flex-wrap__container__ball"
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
    initPosition: {
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
  methods: {
    setNewDestination() {
      console.log("setNewDestination");
      for (let index = 0; index < 9; index++) {
        let element = document.getElementById(`ball${index}`);
        if (element && this.x && this.y) {
          // console.log("this.x", this.x);
          // console.log("this.y", this.y);
          let translateY = this.y - this.initPosition[index].y;
          let translateX = this.x - this.initPosition[index].x;
          this.keyframes[index] = [
            { transform: "translate(-50%, -50%)" },
            { transform: `translate(${translateX}px, ${translateY}px)` },
          ];
        }
      }
    },
    moveBalls() {
      for (let index = 0; index < 9; index++) {
        let element = document.getElementById(`ball${index}`);
        if (element) {
          element.animate(this.keyframes[index], this.animationProperties);
        }
      }
    },
    init() {
      for (let index = 0; index < 9; index++) {
        let element = document.getElementById(`ball${index}`);
        if (element) {
          let rect = element.getBoundingClientRect();
          this.initPosition[index] = { x: rect.left, y: rect.top };
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
  }
  100% {
    transform: translate(250px, -50%);
  }
}
.nine-square-grid {
  overflow: hidden;
  height: 100vh;
  display: flex;
  align-items: center;

  &__flex-wrap {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
    min-width: 100%;
    column-gap: 10px;
    &__container {
      width: calc(33.33% - 24px);
      position: relative;

      &__grid-cell {
        height: 100px;
        margin: 10px 0 10px 0;
        border: black solid 2px;
        background: radial-gradient(
          circle,
          rgba(113, 81, 95, 1) 81%,
          rgba(0, 0, 0, 1) 100%
        );
        &-blink {
          animation: blink 1s infinite;
        }
      }
      &__ball {
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        width: 30px;
        height: 30px;
        background-color: #a5f12b;
        border-radius: 50%;
        position: absolute;
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
}
</style>
