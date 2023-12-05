<template>
  <!-- 點外側區域時關閉 sidebar -->
  <div
    class="backdrop"
    :class="{
      'backdrop-active': showSidebar,
      'backdrop-inactive': !showSidebar,
    }"
    @click.self="toggleSidebar"
  >
    <select name="" id="" @change="selectOnChange" :value="activeKey">
      <option value="">請選擇</option>
      <recursive-option parent="" :data="data"></recursive-option>
    </select>
    <!-- sidebar區塊 -->
    <div class="backdrop__sidebar">
      <sidebar-item
        v-for="(child, index) in data"
        :key="child.key"
        :index="index"
        :activeIndex="activeIndex"
        :activePath="activePath"
        :data="child"
        :setActiveKey="setActiveKey"
        @click="itemOnClick(index)"
        :level="0"
      />
    </div>
  </div>
</template>

<script>
import { fakeData } from "../../fakeData.js";
import RecursiveOption from "./RecursiveOption.vue";
import SidebarItem from "./SidebarItem.vue";

export default {
  components: { SidebarItem, RecursiveOption },
  props: {
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
      activePath: [],
      activeKey: "",
      data: fakeData,
    };
  },
  methods: {
    itemOnClick(index) {
      console.log("itemOnClick");
      this.setActiveIndex(index);
      this.setActiveKey(this.data[index].key);
      this.setActivePath([index]);
      localStorage.setItem("activePath", JSON.stringify([index]));
    },
    initActiveData() {
      console.log("initActiveData");
      this.setActiveIndex(-1);
      this.setActiveKey("");
      this.setActivePath([]);
      localStorage.clear();
    },
    setActiveIndex(index) {
      this.activeIndex = index;
    },
    setActiveKey(key) {
      this.activeKey = key;
      localStorage.setItem("activeKey", key);
    },
    setActivePath(path) {
      this.activePath = path;
      localStorage.setItem("activePath", JSON.stringify(path));
    },
    findKeyRecursively(data, targetKey, path = []) {
      for (let i = 0; i < data.length; i++) {
        let item = data[i];
        let currentPath = [...path, i];

        if (item.key === targetKey) {
          this.setActiveIndex(currentPath[0]);
          this.setActiveKey(targetKey);
          this.setActivePath(currentPath);
        }

        if (item?.children?.length > 0) {
          this.findKeyRecursively(item.children, targetKey, currentPath);
        }
      }
    },
    selectOnChange(event) {
      console.log("selectOnChange");
      localStorage.clear();
      event?.target?.value?.length === 0
        ? this.initActiveData()
        : this.findKeyRecursively(this.data, event.target.value);
    },
  },
  mounted() {
    let storagePath = localStorage.getItem("activePath");
    let storageKey = localStorage.getItem("activeKey");
    if (storagePath && storageKey) {
      console.log("sidebar mounted");
      let parsePath = JSON.parse(storagePath);
      this.setActiveIndex(parsePath[0]);
      this.setActiveKey(storageKey);
      this.setActivePath(parsePath);
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
  &-active {
    // background-color: green;
    transform: translateX(0);
  }
  &-inactive {
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
