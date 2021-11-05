<template>
  <div>123123
<!--    <slot/>-->
  </div>
</template>

<script>
export default {
  name: 'Step1',
  beforeMount() {
    this.$emit('mount', this);
  },
};
</script>

<style scoped>

</style>
