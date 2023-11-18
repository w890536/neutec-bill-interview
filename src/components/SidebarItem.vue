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
        style="padding-left: 20px"
        @click.stop="handleSubClick(idx)"
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
  },
  data() {
    return {
      activeSubIndex: -1,
    };
  },
  methods: {
    isActive() {
      return this.activeIndex === this.index;
    },
    handleClick() {
      this.$emit("clickIndex", this.index);
    },
    handleSubClick(index) {
      this.activeSubIndex = index;
    },
  },
  mounted() {},
};
</script>

<style lang="scss" scoped>
.sidebaritem {
  &--active {
    background-color: gray;
  }
  p {
    text-align: start;
  }
}
</style>
