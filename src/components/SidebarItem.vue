<template>
  <div
    class="sidebaritem"
    @click="handleClick"
    :class="{ 'sidebaritem--active': isActive() }"
  >
    <p :style="{ color: isActive() ? 'yellow' : 'white' }">
      {{ data.text }}
    </p>
    <template v-if="isActive()">
      <sidebar-item
        v-for="(child, idx) in data.children"
        :key="child.key"
        :data="child"
        :index="idx"
        :activeIndex="activeSubIndex"
        :activePath="activePath.slice(1)"
        style="padding-left: 20px"
        @click.stop="handleSubClick(idx)"
        :level="level + 1"
      />
    </template>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      default: () => {},
    },
    index: {
      type: Number,
      default: -1,
    },
    activeIndex: {
      type: Number,
      default: -1,
    },
    activePath: {
      type: Array,
      default: () => [],
    },
    level: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      activeSubIndex: -1,
      activeArray: [],
    };
  },
  watch: {
    activePath: {
      handler(newActivePath) {
        if (newActivePath.length > 0) {
          this.activeSubIndex = newActivePath[0];
        }
      },
      immediate: true,
    },
  },
  methods: {
    isActive() {
      return this.activeIndex === this.index;
    },
    handleClick() {
      this.$emit("clickIndex", this.index);
      this.activeSubIndex = -1;
      localStorage.setItem(`level${this.level}`, -1);
    },
    handleSubClick(index) {
      this.activeSubIndex = index;
      localStorage.setItem(`level${this.level}`, index);
    },
  },
  mounted() {
    if (localStorage.getItem(`level${this.level}`) !== null) {
      this.activeSubIndex = parseInt(
        localStorage.getItem(`level${this.level}`)
      );
    }
  },
};
</script>

<style lang="scss" scoped>
.sidebaritem {
  cursor: pointer;
  &--active {
    background-color: gray;
  }
  p {
    text-align: start;
  }
}
</style>
