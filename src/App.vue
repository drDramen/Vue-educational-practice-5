<template>
  <div id="app">
    <button @click="openModal">modal open</button>
    <hr>
    <modal ref="modal">
      do you want delete?

<!--      <template #footer="{done}">-->
<!--        <button @click="done">OKkk!</button>-->
<!--      </template>-->
    </modal>

    <modal ref="modal1">
      really??
      <template #footer="{done}">
        <button @click="done">OKkk!</button>
      </template>
    </modal>
  </div>
</template>

<script>
import Modal from '@/components/Modal.vue';

export default {
  name: 'App',
  data: () => ({}),
  components: { Modal },
  methods: {
    async openModal() {
      try {
        const model = await this.$refs.modal.open();
        console.log(model.isOpen);
        // if (true) {
        //   throw new Error('qwfewr');
        // }
      } catch (e) {
        console.log(e);
      }
      //   this.$refs.modal1.open()
      //       console.log(data);
      //     })
      //     .catch(() => {
      //       console.log(2);
      //     });
      // })
      // .catch(() => {
      //   console.log(2);
      // });
    },
  },
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
