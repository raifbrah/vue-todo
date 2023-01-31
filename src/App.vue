<template>
  <div class="wrapper">
    <h2 class="title">ToDo List</h2>

    <transition-group
      v-if="list.length > 0"
      name="list"
      tag="ul"
      class="task-list"
    >
      <li v-for="(task, taskIndex) in list" :key="task" class="task-list__task">
        <input
          @click="checkbox(task)"
          :checked="task.checked"
          type="checkbox"
          class="checkbox-input"
        />
        <span
          @click="selectTask(task, taskIndex)"
          :class="{
            'task-list__name_completed': task.checked,
          }"
          class="task-list__name"
        >
          {{ task.name }}</span
        >
      </li>
    </transition-group>

    <add-task-input @add-task="add" />

    <task-context-menu
      @close="closeContextMenu"
      @cancel="cancelContextMenu"
      @do-action="contextMenu_doAction"
      @save-task="saveTask"
      :is-open-context-menu="isOpen.contextMenu"
      :selected-task-name="selectedTask.name"
    />
  </div>
</template>

<script>
import AddTaskInput from "./components/add-task-input.vue";
import TaskContextMenu from "./components/task-context-menu.vue";

export default {
  name: "App",

  components: {
    AddTaskInput,
    TaskContextMenu,
  },

  data() {
    return {
      selectedTaskIndex: null,
      selectedTask: { name: "" },

      isOpen: {
        contextMenu: false,
      },
      list: localStorage.list ? JSON.parse(localStorage.list) : [],
    };
  },

  methods: {
    add(e) {
      const newTask = {
        name: e,
        checked: false,
      };
      this.list = [newTask, ...this.list];

      this.saveToLocalStorage();
    },

    checkbox(task) {
      task.checked = task.checked === true ? false : true;

      const uncompletedTasks = this.list.filter((value) => {
        if (value.checked === false) {
          return true;
        }
      });
      const completedTasks = this.list.filter((value) => {
        if (value.checked === true) {
          return true;
        }
      });

      this.list = [...uncompletedTasks, ...completedTasks];

      this.saveToLocalStorage();
    },

    selectTask(elem, elemIndex) {
      this.isOpen.contextMenu = true;
      this.selectedTaskIndex = elemIndex;
      this.selectedTask = elem;
    },

    closeContextMenu() {
      this.isOpen.contextMenu = false;
    },

    cancelContextMenu() {
      this.selectedTask = { name: "" };
    },

    contextMenu_doAction(actionName) {
      if (actionName === "Remove") {
        this.list.splice(this.selectedTaskIndex, 1);
        this.saveToLocalStorage();
      }
    },

    saveTask(event) {
      this.selectedTask.name = event;
      this.saveToLocalStorage();
    },

    saveToLocalStorage() {
      localStorage.list = JSON.stringify(this.list);
    },
  },
};
</script>

<style scoped>
:global(:root) {
  --bg-color: #000;
  --gray-bg-color: #070707;
  --text-color: #ccc;
  --shadow-bg_color: #333;
  --accent-color: #5c5;
  --border-color: 1px solid #333;
}
:global(*) {
  box-sizing: border-box;
}
:global(body) {
  margin: 0;

  font-size: 20px;
  font-family: sans-serif;

  background: var(--bg-color);

  color: var(--text-color);
}
:global(h1),
:global(h2),
:global(h3),
:global(h4),
:global(h5),
:global(h6) {
  color: #fff;
}

.checkbox-input {
  width: 25px;
  height: 25px;

  flex-shrink: 0;
}

.wrapper {
  /* min-height: 100vh; */
  padding: 10px;
  padding-bottom: 100px;
}

.task-list {
  list-style: none;
  padding: 0;

  display: flex;
  flex-direction: column;
  gap: 25px;
}
.task-list__task {
  display: flex;
  gap: 15px;
  align-items: center;
  transition: 0.5s;
}
.task-list__name {
  position: relative;
  overflow-wrap: anywhere;
  cursor: pointer;
}
.task-list__name_completed {
  opacity: 0.8;
  text-decoration: line-through;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  translate: -30px 0px;
}

.list-leave-active {
  position: absolute;
}

.list-move {
  transition: 0.5s;
}
</style>
