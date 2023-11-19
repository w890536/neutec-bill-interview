<template>
  <template v-for="item in items" :key="item.key">
    <template v-if="parent === ''">
      <option :value="item.key">{{item.text}}</option>
      <RecursiveOption
        :items="item.children"
        parent="-"
        v-if="item.children && item.children.length > 0"
      />
    </template>
    <template v-else>
      <option :value="item.key">{{`${parent}${item.text}`}}</option>
      <RecursiveOption
        :items="item.children"
        :parent="`${parent}-`"
        v-if="item.children && item.children.length > 0"
      />
    </template>
  </template>
</template>

<script>
export default {
  props: {
    items: {
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
