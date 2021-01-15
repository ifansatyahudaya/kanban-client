<template>
  <div class="container main">
    <div class="boards">
      <TasksPerCategory
        v-for="task in tasks" :key="task.id"
        v-bind:availableCategories="getAvailableCategory(task.id)"
        v-bind:task="task"
        v-bind:currentUser="currentUser"
        v-on:addTaskPayload="addTaskPayload"
        v-on:onDelete="onDelete"
        v-on:onUpdateTask="onUpdateTask"
        v-on:onMoveCategory="onMoveCategory">
      </TasksPerCategory>
    </div>
  </div>
</template>

<script>
import TasksPerCategory from './TasksPerCategory'

export default {
  name: 'Home',
  components: {
    TasksPerCategory
  },
  props: ['tasks', 'currentUser'],
  methods: {
    addTaskPayload(payload) {
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

    getAvailableCategory(categoryId) {
      let categories = this.$props.tasks.filter((c) => c.id != categoryId)
      categories = categories.map((c) => { return { id: c.id, name: c.name }})
      return categories
    }
  }

}
</script>

<style>

</style>