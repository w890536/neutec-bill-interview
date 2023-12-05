<template>
  <div
    class="sidebaritem"
    :class="{ 'sidebaritem--active': isActive() }"
    @click="clearSubIndex"
  >
    <p :style="{ color: isActive() ? 'yellow' : 'white' }">
      {{ data.text }}
    </p>
    <template v-if="isActive()">
      <sidebar-item
        style="padding-left: 20px"
        v-for="(child, idx) in data.children"
        :key="child.key"
        :data="child"
        :index="idx"
        :activeIndex="subActiveIndex"
        :activePath="activePath"
        :setActiveKey="setActiveKey"
        @click.stop="handleSubClick(idx, child.key)"
        :level="level + 1"
      />
    </template>
  </div>
</template>

<script>
export default {
  props: {
    setActiveKey: {
      type: Function,
    },
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
      subActiveIndex: -1,
    };
  },
  watch: {
    activePath: {
      handler(newActivePath) {
        console.log("activePath changed");
        if (newActivePath.length > 0) {
          this.subActiveIndex = newActivePath[this.level + 1];
        }
      },
    },
  },
  methods: {
    isActive() {
      return this.activeIndex === this.index;
    },
    clearSubIndex() {
      console.log("clearSubIndex");
      this.subActiveIndex = -1;
    },
    handleSubClick(index, key) {
      console.log("handleSubClick");
      this.subActiveIndex = index;
      this.setActiveKey(key);
      let newPath = this.activePath;
      newPath[this.level + 1] = index;
      localStorage.setItem(
        "activePath",
        JSON.stringify(newPath.slice(0, this.level + 2))
      );
    },
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
