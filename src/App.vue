<template>
  <div id="app">
    <step-form/>
  </div>
</template>

<script>

import StepForm from '@/components/stepForm/StepForm.vue';

export default {
  name: 'App',
  components: { StepForm },
  data() {
    return {};
  },
  computed: {},
  methods: {},

};

</script>

<style lang="sass">
*
  margin: 0
  padding: 0
  box-sizing: border-box

#app
  max-width: 1200px
  margin: 0 auto
  padding: 50px 30px 0
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  //text-align: center
  color: #2c3e50

.actions
  position: relative
  padding: 0
  background-color: #e9ce66

  button
    //position: absolute
    //top: 0
    //left: 0
    //width: 100%
    //height: 100%
    border: none
    cursor: pointer

    &:hover
      color: #fff

//.table__body-row:last-child
//  .table__body-cell:last-child.actions
//    button
//      border-bottom-right-radius: 10px

.actions__wrap
  text-align: center

  button
    display: inline-block

    font-size: 24px

  button + button
    margin-left: 20px

</style>
