<template>
  <div class='board'>
    <div class='board-header'>
      {{ task.name }}
    </div>
    <div class='board-body'>
      <Tasks
        v-for="todo in task.Tasks" :key="todo.id"
        v-bind:todo="todo"
        v-bind:currentUser="currentUser"
        v-bind:availableCategories="availableCategories"
        v-on:onDelete="onDelete"
        v-on:onUpdateTask="onUpdateTask"
        v-on:onMoveCategory="onMoveCategory">
      </Tasks>
    </div>
    <div class='board-add-task-wrapper'>
      <form v-if="isAddingTask" @submit.prevent="addTask">
        <input v-model="name" type="text" class="form-control" placeholder="Task Name..">
        <br />
        <button type="submit" class='btn btn-success'><i class='fa fa-check'></i> Add Task</button>
        <button type="button" v-on:click="clearName" class='btn btn-secondary'><i class='fa fa-times'></i> Cancel</button>
      </form>
      <button v-if="!isAddingTask" v-on:click="startAddTask" class='add-task-btn'>
        <i class="fa fa-plus"></i> Add Task
      </button>
    </div>
  </div>
</template>

<script>
import Tasks from './Tasks'

export default {
  name: 'TaskPerCategory',
  data() {
    return {
      name: '',
      isAddingTask: false
    }
  },
  components: {
    Tasks
  },
  props: ['task', 'currentUser', 'availableCategories'],
  methods: {
    startAddTask() {
      this.isAddingTask = true
    },
    clearName() {
      this.name = ''
      this.isAddingTask = false
    },
    addTask() {
      const payload = {
        name: this.name,
        // Cara Baca props task gmna?
        CategoryId: this.task.id,
        clearName: this.clearName
      }

      this.$emit("addTaskPayload", payload)
    },

    onDelete(id) {
      this.$emit("onDelete", id)
    },

    onUpdateTask(payload, callback) {
      this.$emit("onUpdateTask", payload, callback)
    },

    onMoveCategory(payload) {
      this.$emit("onMoveCategory", payload)
    },
  }

}
</script>

<style>

</style>