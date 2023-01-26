<template>
  <div
    :class="{
      wrapper_open: isOpen,
    }"
    class="wrapper"
  >
    <h2 class="window-title">Editing a task</h2>
    <span
      ref="text-area"
      @input="updateTextArea"
      contenteditable
      class="text-area"
    ></span>

    <div class="buttons">
      <button @click="cancelBtn" class="btn">Cancel</button>
      <button @click="saveTask" class="btn">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditTaskWindow",

  props: {
    isOpen: Boolean,
    selectedTaskName: String,
  },

  methods: {
    saveTask() {
      if (this.$refs["text-area"].innerText.trim() !== "") {
        this.$emit("save-task", this.$refs["text-area"].innerText.trim());
      }
    },

    cancelBtn() {
      this.$emit("cancel");
    },
  },

  watch: {
    selectedTaskName() {
      this.$refs["text-area"].innerHTML = this.selectedTaskName;
    },
  },
};
</script>

<style scoped>
.wrapper {
  z-index: 1000;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  padding: 20px;
  padding-bottom: 75px;
  background: var(--bg-color);

  gap: 15px;
  display: flex;
  flex-direction: column;

  opacity: 0;
  translate: 0px 30px;
  visibility: hidden;

  transition: translate 0.3s, opacity 0.3s, visibility 0.3s;
}
.wrapper_open {
  opacity: 1;
  translate: 0px 0px;
  visibility: visible;
}

.window-title {
  margin: 0;
}

.text-area {
  width: 100%;
  height: max-content;
  max-height: 70vh;
  overflow-y: auto;

  padding: 10px;
  border: 2px solid var(--accent-color);
  border-radius: 7px;
  outline: none;
}

.buttons {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 10px;

  display: flex;
  gap: 15px;
  justify-content: center;

  background: var(--gray-bg-color);
  border-top: var(--border-color);
}
.btn {
  font-size: inherit;
  padding: 7px;
  min-width: 120px;

  cursor: pointer;
}
</style>
