<template>
  <!-- 點外側區域時關閉 sidebar -->
  <div
    class="backdrop"
    :class="{
      'backdrop--active': showSidebar,
      'backdrop--inactive': !showSidebar,
    }"
    @click.self="toggleSidebar"
  >
    <!-- sidebar區塊 -->
    <div class="backdrop__sidebar">
      <sidebar-item
        v-for="(child, idx) in data"
        :index="idx"
        :activeIndex="activeIndex"
        :data="child"
        :key="child.key"
        @clickIndex="updateIndex"
      />
    </div>
  </div>
</template>

<script>
import SidebarItem from "./SidebarItem.vue";
export default {
  components: { SidebarItem },
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    showSidebar: {
      type: Boolean,
      default: false,
    },
    toggleSidebar: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      activeIndex: -1,
    };
  },
  methods: {
    updateIndex(index) {
      console.log("update index ", index);
      this.activeIndex = index;
    },
  },
  mounted() {},
};
</script>

<style lang="scss" scoped>
.backdrop {
  position: fixed;
  width: 100%;
  height: 100vh;
  right: 0%;
  top: 0%;
  transition: transform 0.5s;
  &--active {
    transform: translateX(0);
  }
  &--inactive {
    transform: translateX(100%);
  }
  &__sidebar {
    background-color: #000;
    float: right;
    min-width: 150px;
    height: 100%;
    padding: 10px;
  }
}
</style>
