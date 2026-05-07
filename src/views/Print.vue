<template>
  <main class="print-view">
    <div class="printable-items">
       <button v-for="(item, index) in printableItems" :key="index" @click="addPrintItem(index)">{{item.title}}</button>
    </div>

    <label v-if="hasSpells" class="print-options">
      <input type="checkbox" v-model="noFluff"> Hide spell flavor text
    </label>

    <p>Click what you would like to print in the box above to add it to the preview below, then <a href="javascript:window.print()">print</a>.</p>

    <PrintItems :noFluff="noFluff" />
  </main>
</template>

<script>
import PrintItems from '@/components/Print/PrintItems';
import store from '@/store';

export default {
  name: 'PrintView',
  components: { PrintItems },
  data: () => ({ noFluff: false }),
  computed: {
    printableItems: () => store.getters.printableItems,
    hasSpells: () => Array.isArray(store.getters.spells) && store.getters.spells.length > 0
  },
  methods: {
    addPrintItem (index) {
      store.dispatch('addPrintItem', index);
    }
  }
};
</script>

<style lang="scss">
  .print-view {
    .printable-items,
    .print-items {
      border: .1rem dotted $_color_dark;
      margin: 0 0 ($_ / 2);
      min-height: 2 * $_;
      padding: (($_ / 4) - .1rem) ($_ / 2);
    }

    .print-items {
      @include padding((($_ / 2) - .1rem) null);

      margin: 0;
    }

    button {
      @include padding(0 null);

      font-size: 1.2rem;
      line-height: $_ - .2rem;
      margin: ($_ / 4) .5em ($_ / 4) 0;
    }

    .print-options {
      display: block;
      margin: 0 0 ($_ / 2);
    }

    @media print {
      .print-options {
        display: none;
      }
    }
  }
</style>
