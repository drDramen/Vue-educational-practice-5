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
      <slot name="footer" :done="done">
        <button @click="cancel">Cancel</button>
        <button @click="done">OK</button>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  data: () => ({
    isOpen: false,
    resolve: null,
    reject: null,
  }),
  props: {
    // isOpen: {
    //   default: false,
    // },
  },
  methods: {
    open() {
      this.isOpen = true;

      return new Promise((resolve, reject) => {
        this.resolve = resolve;
        this.reject = reject;
      });
    },

    cancel() {
      this.hide();
      this.reject('cancel');
    },

    done() {
      this.hide();
      this.resolve(this);
    },

    hide() {
      this.isOpen = false;
    },
  },
};
</script>

<style scoped>

</style>
