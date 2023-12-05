<template>
  <button class="btn" @click="toggleSidebar">Toggle Btn</button>
  <div class="input-area">
    <label for="x">輸入希望小球移動到x座標: </label>
    <input v-model="x" name="x" type="number" />
    <br />
    <label for="y">輸入希望小球移動到y座標: </label>
    <input v-model="y" name="y" type="number" />
    <br />
    <br />
    <button @click="startMoving">移動</button>
  </div>
  <sidebar :showSidebar="showSidebar" :toggleSidebar="toggleSidebar"></sidebar>
  <nine-square-grid
    :x="x"
    :y="y"
    :gridData="gridData"
    ref="nine"
  />
</template>

<script>
import Sidebar from "./components/Sidebar/Sidebar.vue";
import NineSquareGrid from "./components/NineSquareGrid/NineSquareGrid.vue";

export default {
  components: { Sidebar, NineSquareGrid },
  data() {
    return {
      x: null,
      y: null,
      // 0 一般 // 1 發亮 // 2 球 // 3 發亮且有球
      gridData: [2, 0, 3, 0, 1, 0, 2, 0, 3],
      showSidebar: false,
    };
  },
  methods: {
    toggleSidebar() {
      this.showSidebar = !this.showSidebar;
    },
    startMoving() {
      this.$refs.nine.setNewDestination();
    },
  },
};
</script>

<style lang="scss" scoped>
.btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #fff;
  color: black;
}
.input-area {
  position: absolute;
  top: 100px;
  margin: 0 auto;
  width: 100%;
  input {
    background-color: #fff;
    color: black;
  }
}
</style>
