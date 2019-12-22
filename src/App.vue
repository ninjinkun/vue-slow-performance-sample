<template>
  <div class="row">
    <div class="column">
      <h2>1. Immutableに全Itemを更新（遅い）</h2>
      <table class="table">
        <tr v-for="(itemArray, index) in chunk(immutableItems, 10)" :key="index">
          <td v-for="item in itemArray" :key="item.key" class="cell">
            <ItemCell :item="item" @check="immutableUpdate(item, $event)" />
          </td>
        </tr>
      </table>
    </div>
    <div class="column">
      <h2>2. 直接1アイテムのみを更新（速い）</h2>
      <table class="table">
        <tr v-for="(itemArray, index) in chunk(mutableItems, 10)" :key="index">
          <td v-for="item in itemArray" :key="item.key" class="cell">
            <ItemCell :item="item" @check="mutableUpdate(item, $event)" />
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import { cloneDeep, chunk } from "lodash-es";
import ItemCell from "./components/ItemCell.vue";

export default {
  name: "app",
  components: {
    ItemCell
  },
  data() {
    const items = [...Array(10000)].map((_, index) => ({
      id: `${index}`,
      name: `${index}`,
      checked: false
    }));
    return {
      immutableItems: cloneDeep(items),
      mutableItems: cloneDeep(items)
    };
  },
  methods: {
    chunk,
    immutableUpdate(item, checked) {
      this.immutableItems = this.immutableItems.map(i =>
        i.id === item.id ? { ...i, checked } : { ...i }
      );
    },
    mutableUpdate(item, checked) {
      item.checked = checked;
    }
  }
};
</script>

<style>
.table {
  border-collapse: collapse;
}

.cell {
  border: 1px solid grey;
}

.row {
  display: flex;
}

.column {
  width: 50%;
  border: 1px solid grey;
  padding: 1em;
}
</style>
