<template>
  <template v-for="item in data" :key="item.key">
    <option :value="item.key">
      {{ parent === "" ? item.text : `${parent}${item.text}` }}
    </option>
    <RecursiveOption
      v-if="item.children && item.children.length > 0"
      :data="item.children"
      :parent="`${parent}>`"
    />
  </template>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    parent: {
      type: String,
      default: "",
    },
  },
};
</script>
