<template>
  <div class="wrapper">
    <ul
      :class="{
        'context-menu_open': this.isOpen.contextMenu,
      }"
      class="context-menu"
    >
      <li
        v-for="item in items"
        :key="item"
        @click="goNext(item)"
        class="context-menu__item"
      >
        {{ item }}
      </li>
    </ul>

    <span
      @click="closeAll"
      :class="{
        'shadow-bg_open': shadowBgIsOpen,
      }"
      class="shadow-bg"
    ></span>
  </div>

  <confirmation-window
    @cancel="closeAll"
    @confirm="doAction"
    :is-open="isOpen.confirmationWindow"
    :title="confirmationWindow_action"
  />

  <edit-task-window
    @cancel="closeAll"
    @save-task="saveTask"
    :selected-task-name="selectedTaskName"
    :is-open="isOpen.editTaskWindow"
  />
</template>

<script>
import ConfirmationWindow from "./confirmation-window.vue";
import EditTaskWindow from "./edit-task-window.vue";

export default {
  name: "TaskContextMenu",

  props: {
    isOpenContextMenu: Boolean,
    selectedTaskName: String,
  },

  components: {
    ConfirmationWindow,
    EditTaskWindow,
  },

  data() {
    return {
      isOpen: {
        contextMenu: this.takeIsOpenContextMenu,
        confirmationWindow: false,
        editTaskWindow: false,
      },

      items: ["Edit", "Remove"],

      confirmationWindow_action: null,
    };
  },

  emits: ["close", "cancel", "doAction", "save-task"],

  methods: {
    closeAll() {
      for (let objName in this.isOpen) {
        if (objName !== "contextMenu") {
          this.isOpen[objName] = false;
        }
      }

      this.$emit("close");
      this.$emit("cancel");
    },

    goNext(elem) {
      if (elem === "Remove") {
        this.isOpen.confirmationWindow = true;
      } else if (elem === "Edit") {
        this.isOpen.editTaskWindow = true;
      }
      this.$emit("close");
      this.confirmationWindow_action = elem;
    },

    doAction() {
      this.$emit("doAction", this.confirmationWindow_action);
      this.closeAll();
    },

    saveTask(event) {
      this.$emit("save-task", event);
      this.closeAll();
    },
  },

  computed: {
    shadowBgIsOpen() {
      let foo = false;

      for (let objName in this.isOpen) {
        if (this.isOpen[objName] === true) {
          foo = true;
        }
      }

      return foo;
    },
  },

  watch: {
    isOpenContextMenu() {
      this.isOpen.contextMenu = this.isOpenContextMenu;
    },
  },
};
</script>

<style scoped>
.wrapper {
  --transition: 200ms;
}
.context-menu {
  z-index: 1000;
  position: fixed;
  left: 50%;
  bottom: 20px;
  translate: -50% calc(100% + 25px);

  gap: 2px;
  display: flex;
  flex-direction: column;

  width: calc(100vw - 40px);
  max-width: 320px;

  margin: 0;
  padding: 10px 0px;
  background: #000;
  list-style: none;
  border-radius: 3px;

  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition: translate var(--transition), opacity var(--transition),
    visibility var(--transition);
}
.context-menu_open {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  translate: -50% 0px;
}

.context-menu__item {
  padding: 10px 20px;
  cursor: pointer;
}
.context-menu__item:hover {
  background: #3332;
}

.shadow-bg {
  z-index: 900;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: -50vh;

  background: var(--shadow-bg_color);

  opacity: 0;
  visibility: hidden;
  transition: opacity var(--transition), visibility var(--transition);
}
.shadow-bg_open {
  opacity: 0.9;
  visibility: visible;
}
</style>
