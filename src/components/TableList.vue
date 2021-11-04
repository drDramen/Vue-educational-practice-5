<template>
  <table class="table">
    <tr class="table__header-row">
      <th
        v-for="header in headers"
        :key="`header-${header.key}`"
        @click="sort(header)"
        class="table__header-cell"
        :class="{
          active: header.key === activeHeaderKey,
          sortable: !!header.sort
        }"
      >
        <slot
          v-if="!$scopedSlots[`header-${header.key}`]"
          name="header"
          :header="header"
          style="display: inline-block;"
        >
          {{ header.label || header.key }}
        </slot>
        <slot v-else :name="`header-${header.key}`" :header="header" style="display: inline-block;">
          {{ header.label || header.key }}
        </slot>
        <span
          v-if="header.sort"
          class="arrow"
          :class="header.sort.direction > 0 ? 'asc' : 'dsc'"
        ></span>
      </th>
    </tr>
    <tr v-for="item in items" :key="item.id" class="table__body-row">
      <td
        v-for="header in headers"
        :key="`header-${header.key}`"
        class="table__body-cell"
        :class="header.class"
        :contenteditable="header.editable"
        @blur="item[header.key] = $event.target.innerHTML.trim()"
      >
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
  props: {
    columns: { type: Array, default: () => [] },
    items: { type: Array, default: () => [] },
    sortingHeaderKey: String,
  },
  data() {
    return {
      currentHeaderKey: this.sortingHeaderKey,
    };
  },
  computed: {
    activeHeaderKey() {
      return this.sortingHeaderKey || this.currentHeaderKey;
    },
    headers() {
      const { items } = this;
      let headers = [];

      const headersFromItems = () => Object.keys(items[0] || {}).map((h) => ({ key: h, label: h }));

      if (this.columns.length) {
        const additional = this.columns.filter((h) => this.type(h) === 'object' && Object.prototype.hasOwnProperty.call(h, 'additional'));
        if (this.columns.length !== additional.length) {
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
          return headers;
        }
        headers = headersFromItems();
        if (additional.length) {
          additional.forEach((ah) => {
            const index = headers.indexOf((h) => h.key === ah.key);

            if (index >= 0) {
              headers[index] = ah;
            } else {
              headers.push(ah);
            }
          });
        }
      } else {
        headers = headersFromItems();
      }
      return headers;
    },
  },
  methods: {
    type(value) {
      const regex = /^\[object (\S+?)]$/;
      const matches = Object.prototype.toString.call(value).match(regex) || [];

      return (matches[1] || 'undefined').toLowerCase();
    },
    sort(header) {
      if (Object.prototype.hasOwnProperty.call(header, 'sort')) {
        this.currentHeaderKey = header.key;
        this.$emit('sort', this.currentHeaderKey);
      }
    },
  },
};

</script>

<style lang="sass">
.table
  width: 100%
  border-radius: 10px
  border-collapse: collapse

  &__header-cell, &__body-cell
    padding: 5px 10px
    border-style: solid
    border-width: 0 2px 2px 0
    border-color: #fff

  &__header-cell
    color: #dbd9d9
    background-color: #5291db

    &.sortable
      cursor: pointer

      &.active
        color: #fff

        .arrow
          opacity: 1

    &:first-child
      border-top-left-radius: 10px

    &:last-child
      border-top-right-radius: 10px
      border-right: none

  &__body-cell
    background-color: rgba(248, 227, 145, .7)

  &__body-row:hover
    .table__body-cell
      background-color: #e9ce66

  &__body-row:last-child
    .table__body-cell
      border-bottom: none

      &:first-child
        border-bottom-left-radius: 10px

      &:last-child
        border-bottom-right-radius: 10px

.arrow
  display: inline-block
  vertical-align: middle
  width: 0
  height: 0
  margin-left: 5px
  opacity: 0.5

  &.asc
    border-left: 5px solid transparent
    border-right: 5px solid transparent
    border-bottom: 5px solid #fff

  &.dsc
    border-left: 5px solid transparent
    border-right: 5px solid transparent
    border-top: 5px solid #fff

</style>
