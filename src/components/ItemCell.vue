<template>
  <div :class="[{ checked: item.checked }]">
    <div class="name">{{ item.name }}</div>
    <input type="checkbox" :value="item.checked" @change="input" />
  </div>
</template>

<script>
export default {
  props: {
    item: Object
  },
  methods: {
    input(event) {
      this.$emit("check", event.target.checked, this.item.id);
    }
  }
};
</script>

<style>
.checked {
  background: green;
}

.name {
  white-space: nowrap;
}
</style>