<template>
  <div>
    step2
  </div>
</template>

<script>
export default {
  name: 'Step2',
  beforeMount() {
    this.$emit('mount', this);
  },
};
</script>

<style scoped>

</style>
