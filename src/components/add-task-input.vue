<template>
  <div class="add-task-wrapper">
    <div :class="{ 'add-task-input_open': opened }" class="add-task-input">
      <div
        ref="ati__textarea"
        @input="update"
        @keyup.enter="add"
        class="ati__textarea"
        contenteditable="true"
        id="ati__textarea"
      ></div>
      <div class="ati__footer">
        <button @click="add" class="btn ati__btn">Add Task</button>
      </div>
    </div>

    <span
      @click="opened = false"
      :class="{ 'shadow-bg_open': opened }"
      class="shadow-bg"
    />

    <span
      @click="opened = true"
      :disabled="opened"
      :class="{ 'add-task-btn_hide': opened }"
      class="add-task-btn"
    ></span>
  </div>
</template>

<script>
export default {
  name: "add-task-input",

  data() {
    return {
      opened: false,
    };
  },

  methods: {
    add() {
      if (this.$refs.ati__textarea.innerText.trim() !== "") {
        this.$emit("add-task", this.$refs.ati__textarea.innerText.trim());
        this.$refs.ati__textarea.innerHTML = "";
        this.opened = false;
      }
    },
  },

  watch: {
    opened(value) {
      if (value === true) {
        setTimeout(() => {
          this.$refs.ati__textarea.focus();
        }, 100);
      }
    },
  },
};
</script>

<style scoped>
.add-task-wrapper {
  --transition: 200ms;
}

.add-task-input {
  z-index: 1000;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;

  background: var(--bg-color);
  padding: 10px;
  border-radius: 5px 5px 0 0;

  pointer-events: none;
  opacity: 0;
  visibility: hidden;
  translate: 0px 30px;
  transition: translate var(--transition), opacity var(--transition),
    visibility var(--transition);
}
.add-task-input_open {
  pointer-events: auto;
  opacity: 1;
  visibility: visible;
  translate: 0px 0px;
}

.ati__footer {
  display: flex;
  justify-content: end;
}
.ati__textarea {
  width: 100%;
  height: max-content;

  padding: 10px;
  border: 2px solid var(--accent-color);
  border-radius: 7px;
  outline: none;
}

.shadow-bg {
  z-index: 900;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  pointer-events: none;
  opacity: 0;
  visibility: visible;
  background: var(--shadow-bg_color);

  transition: opacity var(--transition), visibility var(--transition);
}
.shadow-bg_open {
  pointer-events: auto;
  opacity: 0.9;
  visibility: visible;
}

.add-task-btn {
  font-size: inherit;
  z-index: 800;
  position: fixed;
  right: 10px;
  bottom: 10px;

  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: var(--accent-color);

  font-size: 50px;

  display: flex;
  align-items: center;
  justify-content: center;

  cursor: pointer;
  box-shadow: 0px 4px 10px #3339;

  will-change: translate;
  transition: translate var(--transition);
}
.add-task-btn::before,
.add-task-btn::after {
  content: "";
  position: fixed;
  top: 50%;
  left: 50%;
  translate: -50% -50%;

  width: 25px;
  height: 3px;
  background: #000;
}
.add-task-btn::after {
  width: 3px;
  height: 25px;
}
.add-task-btn_hide {
  translate: 0px calc(15px + 100%);
}

.btn {
  font-size: inherit;
  padding: 7px;
  min-width: 120px;

  cursor: pointer;
}
</style>
