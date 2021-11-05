<template>
  <div>
    <div class="header">
      <slot name="header"/>
    </div>

    <div class="step">
      <template v-for="(step, key) in steps">
        <component :is="step" :key="key" v-if="index === key"/>
      </template>
    </div>

    <div class="footer">
      <slot name="footer">
        <button>Prev</button>
        <button>Next</button>
      </slot>
    </div>
  </div>
</template>

<script>
import Step1 from '@/components/stepForm/Step1.vue';
import Step2 from '@/components/stepForm/Step2.vue';
import Step3 from '@/components/stepForm/Step3.vue';

export default {
  name: 'StepForm',
  data: () => ({
    steps: [
      Step1, Step2, Step3,
    ],
    index: 0,
  }),
  methods: {},
};
</script>

<style scoped>

</style>
