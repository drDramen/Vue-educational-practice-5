<template>
  <table border="1">
    <tr>
      <th v-for="header in headers" :key="`header-${header.key}`">
        <slot v-if="!$scopedSlots[`header-${header.key}`]" name="header" :header="header">
          {{ header.label || header.key }}
        </slot>
        <slot v-else :name="`header-${header.key}`" :header="header">
          {{ header.label || header.key }}
        </slot>
      </th>
    </tr>
    <tr v-for="item in items" :key="item.id">
      <td v-for="header in headers" :key="`header-${header.key}`">
        <slot
          :name="header.key"
          :item="item"
          :header="header"
        >
          {{ item[header.key] }}
        </slot>
      </td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'TableList',
  data() {
    return {
      sorting: {
        currentKey: null,
        order: {},
      },
    };
  },
  props: {
    columns: { type: Array, default: () => [] },
    items: { type: Array, default: () => [] },
  },
  computed: {

    // sortedItems() {
    // },
    headers() {
      let headers = [];
      if (this.columns.length) {
        this.columns.forEach((h) => {
          if (this.type(h) === 'string') {
            headers.push(
              {
                key: h,
                label: h,
              },
            );
          }
          if (this.type(h) === 'object' && Object.prototype.hasOwnProperty.call(h, 'key')) {
            headers.push(h);
          }
        });
      } else {
        headers = Object.keys(this.items[0])
          .map((h) => (
            {
              key: h,
              label: h,
            }
          ));
      }
      this.sortOrder();
      return headers;
    },
  },
  methods: {
    type(value) {
      const regex = /^\[object (\S+?)]$/;
      const matches = Object.prototype.toString.call(value).match(regex) || [];

      return (matches[1] || 'undefined').toLowerCase();
    },
    sort(key) {
      this.sorting.currentKey = key;
    },
    sortOrder() {
      const sortingHeaders = {};
      this.headers.forEach((h) => {
        if (h.sort) {
          sortingHeaders.h = 1;
        }
      });
      this.sorting.order = sortingHeaders;
    },
  },
};

</script>

<style lang="sass">
.table-head
  padding: 5px 10px

//.table
//  text-align: center
//  padding: 20px
//  border: 1px solid #53e3b0
//
//  &__row
//    display: flex
//
//    & + &
//      .table__col
//        border-top: none
//
//  &__col
//    flex: 1 0 0
//    max-width: 25%
//    padding: 5px 10px
//    border: 1px solid #000
//    overflow: hidden
//
//    & + &
//      border-left: none
//
//  &__header &__col
//    text-transform: uppercase
//    font-weight: bold

</style>
