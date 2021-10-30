<template>
  <div v-if="isOpen">
    <div class="header">
      <slot name="header">
        Header
      </slot>
    </div>
    <div class="content">
      <slot></slot>
    </div>
    <div class="footer">
      <slot name="footer">
        <button @click="cancel">Cancel</button>
        <button @click="ok">OK</button>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  data: () => ({
    // isOpen: false,
  }),
  props: {
    isOpen: {
      default: false,
    },
  },
  methods: {
    ok() {
      this.$emit('ok');
    },
    cancel() {
      this.$emit('cancel');
    },
  },
};
</script>

<style scoped>

</style>
