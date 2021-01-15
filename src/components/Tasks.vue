<template>
  <div class='card custom-card' v-bind:class="[todo.UserId == currentUser.id ? 'active' : '']">
    <span v-if="isEditing">
      <textarea class="form-control" v-model="name" />
      <br />
      <button type="button" v-on:click="onCancelEdit" class='btn btn-sm btn-success'><i class='fa fa-check'></i> Edit Task</button>
      <button type="button" v-on:click="cancelEdit" class='btn btn-sm btn-secondary'><i class='fa fa-times'></i> Cancel</button>
    </span>
    <span v-else>
      {{ todo.name }}
    </span>
    <div class='custom-card-footer'>
      <div class="custom-card-footer-info">
        By: {{ todo.User.name }}
        <br>
        {{ todo.createdAt }}
      </div>
      <div v-if="!isEditing" class="custom-card-footer-info hide">
        <span v-on:click="onEdit" class="card-button">
          <i class="fa fa-pen"></i>
        </span>
        <span v-on:click="onDelete" class="card-button">
          <i class="fa fa-trash"></i>
        </span>
        <span class="card-button dropdown-toggle" href="#" id="moveCategory" role="button" data-bs-toggle="dropdown" aria-expanded="false">
          <i class="fa fa-arrows-alt"></i>
        </span>
        <ul class="dropdown-menu" aria-labelledby="moveCategory">
          <li v-for="category in availableCategories" :key="category.id">
            <a class="dropdown-item" v-on:click="moveCategory(category.id)" href="javascript:void(0);">{{ category.name }}</a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
  name: 'Tasks',
  props: ['todo', 'currentUser', 'availableCategories'],
  data() {
    return {
      isEditing: false,
      name: null
    }
  },
  methods: {
    onCancelEdit() {
      this.$emit("onUpdateTask", {
        id: this.$props.todo.id,
        CategoryId: this.$props.todo.CategoryId,
        name: this.name
      }, this.cancelEdit)

      Swal.fire({
        icon: 'success',
        text: 'Task has been updated!'
      })
    },
    cancelEdit() {
      console.log(this.$props.availableCategories)
      this.isEditing = false,
      this.name = null
    },
    onDelete() {
      if (this.todo.UserId == this.currentUser.id) {
        Swal.fire({
          title: 'Are you sure?',
          text: "You won't be able to revert this!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, delete it!'
        }).then((result) => {
          if (result.isConfirmed) {
            Swal.fire(
              'Deleted!',
              'Your file has been deleted.',
              'success'
            )
            this.$emit("onDelete", this.$props.todo.id)
          }
        })

      } else {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'You dont have authorize to do that!'
        })
      }
    },
    onEdit() {
      if (this.todo.UserId == this.currentUser.id) {
        this.isEditing = true
        this.name = this.todo.name
      }
    },
    moveCategory(categoryId) {
      console.log(this.$props.todo.id)
      this.$emit("onMoveCategory", {
        id: this.$props.todo.id,
        CategoryId: categoryId
      })

      Swal.fire({
        icon: 'success',
        title: 'Move task success!'
      })
    }
  }

}
</script>

<style>

</style>