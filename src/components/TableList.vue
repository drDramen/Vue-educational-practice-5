<template>
  <div class="table">

    <div class="table__row table__header">
      <slot
        v-for="(field, idx) in fields"
        :field="field"
        :myKey="idx"
        name="mainHeadField"
        myClass="table__col"
      ></slot>
      <slot name="addField" myClass="table__col"></slot>
    </div>

    <div class="table__body">
      <div v-for="item in items" :key="item.id" class="table__row">
        <slot
          v-for="(field, idx) in fields"
          :item="item"
          :field="field"
          :myKey="idx"
          name="mainHeadField"
          myClass="table__col"
        ></slot>
        <slot :itemIndex="item.id" myClass="table__col" name="addField"></slot>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'TableList',
  props: {
    fields: { type: Array, required: true },
    items: Array,
  },
};

</script>

<style lang="sass">
.table
  text-align: center
  padding: 20px
  border: 1px solid #53e3b0

  &__row
    display: flex

    & + &
      .table__col
        border-top: none

  &__col
    flex: 1 0 0
    max-width: 25%
    padding: 5px 10px
    border: 1px solid #000
    overflow: hidden

    & + &
      border-left: none

  &__header &__col
    text-transform: uppercase
    font-weight: bold

</style>
