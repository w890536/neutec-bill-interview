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
    <select name="" id="" @change="handleSelect">
      <recursive-option parent="" :items="data"></recursive-option>
    </select>
    <!-- sidebar區塊 -->
    <div class="backdrop__sidebar">
      <sidebar-item
        v-for="(child, idx) in data"
        :index="idx"
        :activeIndex="activeIndex"
        :activePath="activeArray"
        :data="child"
        :key="child.key"
        @clickIndex="updateIndex"
        :level="0"
      />
    </div>
  </div>
</template>

<script>
import RecursiveOption from "./RecursiveOption.vue";
import SidebarItem from "./SidebarItem.vue";
export default {
  components: { SidebarItem, RecursiveOption },
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
      default: null,
    },
  },
  data() {
    return {
      activeIndex: -1,
      activeArray: [],
    };
  },
  methods: {
    updateIndex(index) {
      this.activeIndex = index;
      localStorage.setItem("activeIndex", index);
      console.log("update Index ", index);
    },
    findKeyRecursively(data, targetKey, path = [], result = []) {
      for (let i = 0; i < data.length; i++) {
        const item = data[i];
        const currentPath = [...path, i];

        if (item.key === targetKey) {
          result = currentPath;
          this.activeIndex = result.shift();
          this.activeArray = result;
        }

        if (item.children && item.children.length > 0) {
          this.findKeyRecursively(
            item.children,
            targetKey,
            currentPath,
            result
          );
        }
      }
    },
    handleSelect(event) {
      this.findKeyRecursively(this.data, event.target.value);
    },
  },
  mounted() {
    if (localStorage.getItem("activeIndex") !== null) {
      this.activeIndex = parseInt(localStorage.getItem("activeIndex"));
    }
  },
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
  opacity: 0.9;
  z-index: 3;
  &--active {
    // background-color: green;
    transform: translateX(0);
  }
  &--inactive {
    // background-color: blue;
    transform: translateX(100%);
  }
  select {
    margin: 10px;
  }
  &__sidebar {
    background-color: #000;
    float: right;
    width: 150px;
    height: 100%;
    padding: 10px;
  }
}
</style>
