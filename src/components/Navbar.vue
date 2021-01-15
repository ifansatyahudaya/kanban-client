<template>
  <nav class="navbar navbar-dark custom-nav">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <strong>KANBAN</strong>
      </a>
      <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" role="button" data-bs-toggle="dropdown" aria-expanded="false">
        {{ currentUser.name }}

      </a>
      <ul class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
        <li v-if="currentPage != 'login' && currentPage != 'register'">
          <a v-on:click="logout" class="dropdown-item" href="#">Logout</a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  props: ['currentUser', 'currentPage'],
  methods: {
    logout() {
      this.$emit("logout")
    }

  }
}
</script>

<style>

</style>