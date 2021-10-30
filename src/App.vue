<template>
  <div id="app">
    <button @click="modalOpen = !modalOpen">modal open</button>
    <hr>
    <modal @ok="()=>{}" @cancel="()=>{}" :is-open="modalOpen">
      do you want delete?
      <template #footer>
        <button>OKkk!</button>
      </template>
    </modal>
  </div>
</template>

<script>
import Modal from '@/components/Modal.vue';

export default {
  name: 'App',
  data: () => ({
    modalOpen: false,
  }),
  components: { Modal },
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
  text-align: center
  color: #2c3e50

.editBlock
  padding: 30px 20px
  text-align: left

  input, button
    display: inline-block
    margin-left: 10px
    padding: 5px 10px

</style>
