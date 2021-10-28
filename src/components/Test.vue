<template>
  <div @click="click" v-on="$listeners">
    123
    <input type="text" :style="style">
  </div>
</template>

<script>
export default {
  name: 'Test',
  data: () => ({
    edit: false,
  }),
  computed: {
    style() {
      return { display: this.edit ? 'block' : 'none' };
    },
  },
  methods: {
    click() {
      this.edit = !this.edit;
    },
  },
};
</script>

<style scoped>

</style>
