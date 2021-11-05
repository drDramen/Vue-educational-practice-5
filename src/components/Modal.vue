<template>
  <div v-if="isOpen" @click.stop="hide" class="modal">
    <div class="modal__body">
      <div class="modal__header">
        <slot name="header">
          Заголовок
        </slot>
      </div>
      <div class="modal__content">
        <slot>
          Контент
        </slot>
      </div>
      <div class="modal__footer">
        <slot name="footer">
          <button @click="cancel" type="button">Cancel</button>
          <button @click="done" type="button">Ok</button>
        </slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modal',
  data() {
    return {
      isOpen: false,
      resolve: null,
      reject: null,
    };
  },
  methods: {
    open() {
      this.isOpen = true;

      return new Promise((resolve, reject) => {
        this.resolve = resolve;
        this.reject = reject;
      });
    },

    done() {
      this.resolve(this);
      this.hide();
    },
    cancel() {
      this.reject(this);
      this.hide();
    },
    hide() {
      this.isOpen = false;
    },
  },
};
</script>

<style lang="sass" scoped>
.modal
  position: fixed
  top: 0
  left: 0
  display: flex
  justify-content: center
  align-items: center
  width: 100vw
  height: 100%
  //visibility: hidden
  //opacity: 0
  overflow: hidden
  background-color: rgba(0, 0, 0, 0.8)
  z-index: 10

  &__body
    padding: 50px
    background-color: #fff

  &__header
    margin-bottom: 20px

  &__content
    margin-bottom: 20px

  &__footer
    button
      display: inline-block
      padding: 5px 10px
      width: 70px

    button + button
      margin-left: 20px
</style>
