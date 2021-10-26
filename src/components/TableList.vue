<template>
  <div class="table">
    <template v-for="(field, idx) in fields">
      <div :key="idx" class="table__column">
        <div class="table__header">
          <div class="table__cell">
            <slot
              name="table-header"
              :field="field"
            >
            </slot>
          </div>

        </div>
        <div class="table__body">
          <slot
            name="table-body"
            v-for="item in items"
            :myKey="item.id"
            :item="item"
            :field="field"
          >
          </slot>
        </div>
      </div>
    </template>

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
  display: flex
  justify-content: space-between
  text-align: center
  padding: 20px
  border: 1px solid #53e3b0

  &__column
    flex-direction: column
    flex: 1 0 0

    & + &
      .table__cell
        border-left: none

    .table__cell + .table__cell
      border-top: none

  &__header &__cell
    text-transform: uppercase
    font-weight: bold

  &__cell
    padding: 5px 10px
    border: 1px solid #000

</style>
