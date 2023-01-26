<template>
  <div
    :class="{
      wrapper_open: isOpen,
    }"
    class="wrapper"
  >
    <h4 class="title">{{ title }} task?</h4>
    <div class="buttons">
      <button @click="this.$emit('confirm')" class="btn confirm">
        Подтвердить
      </button>
      <button @click="this.$emit('cancel')" class="btn cancel">Отменить</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ConfirmationWindow",

  props: {
    isOpen: Boolean,
    title: String,
  },
};
</script>

<style scoped>
.wrapper {
  --transition: 200ms;

  z-index: 1000;
  position: fixed;
  left: 50%;
  bottom: 20px;
  translate: -50% calc(100% + 25px);

  gap: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;

  width: calc(100vw - 40px);
  max-width: 350px;

  padding: 15px;
  background: var(--bg-color);
  border-radius: 3px;

  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition: translate var(--transition), opacity var(--transition),
    visibility var(--transition);
}
.wrapper_open {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  translate: -50% 0px;
}

.title {
  margin: 0;
}

.buttons {
  display: flex;
  gap: 15px;
}
.btn {
  font-size: inherit;
  padding: 7px;
  min-width: 120px;

  cursor: pointer;
}
</style>
