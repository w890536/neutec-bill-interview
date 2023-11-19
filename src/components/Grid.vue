<template>
  <section>
    <div class="container" v-for="(value, index) in gridData" :key="index">
      <div
        class="container__box"
        :class="{ container__box__blink: value === 1 || value === 3 }"
      />
      <div v-if="value === 2 || value === 3" class="container__ball">0</div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    gridData: {
      type: Array,
      default: () => [],
    },
  },
  methods: {},
  mounted() {},
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
section {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin-top: 200px;
  min-width: 100%;
  column-gap: 10px;
  overflow: hidden !important;
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
      width: 30px;
      height: 30px;
      background-color: #a5f12b;
      border-radius: 50%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      align-items: center;
      justify-content: center;
      transition: transform 0.5s;
      animation: moveball 1s infinite;
      z-index: 2;
    }
  }
}
</style>
