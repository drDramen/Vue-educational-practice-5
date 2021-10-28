<template>
  <div class="table">
    <table border="1">
      <tr>
        <template v-for="(header, key) in headers">
          <th :key="key" v-if="header.sort" @click="header.sort">
            <slot :name="`header-${header.key}`" :header="header">
              {{ header.label }}
            </slot>
            <span>sort</span>
          </th>
          <th :key="key" v-else>
            <slot :name="`header-${header.key}`" :header="header">
              {{ header.label }}
            </slot>
          </th>
        </template>
      </tr>
      <tr v-for="item in items" :key="item.id">
        <td v-for="(header, index) in headers"
            :key="`${index}-${item.id}`"
            :contenteditable="header.editable"
            v-on="header.events">
          <slot :name="header.key"
                :item="item"
                :field="item[header.key]"
                :events="header.events"
                :fieldName="header.key"
                :editable="header.editable || false">
            {{ item[header.key] }}
          </slot>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TableList',
  props: {
    columns: {
      type: Array,
      default: () => [],
    },
    items: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    headers() {
      // TODO: additional functional
      const additional = this.columns.filter((c) => c.additional);
      const headers = Object.keys(this.items[0] || {})
        .map((h) => ({
          key: h,
          label: h,
        }));

      if (this.columns.length && this.columns.length !== additional.length) {
        return this.columns;
      }

      if (additional.length) {
        additional.forEach((additionalHead) => {
          const head = headers.find((h) => h.key === additionalHead.key);

          if (head) {
            const headIndex = headers.indexOf(head);
            headers[headIndex] = additionalHead;
          } else {
            headers.push(additionalHead);
          }
        });
      }

      return headers;
    },
  },
};

</script>

<style lang="sass">
</style>
