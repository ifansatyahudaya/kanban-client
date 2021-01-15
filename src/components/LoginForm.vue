<template>
  <div id="form-login">
    <h1>Login Form</h1>
    <form @submit.prevent="login">
      <div class="form-group">
        <input v-model="email" type="email" placeholder="Email" autofocus="autofocus" class="form-control">
      </div>
      <div class="form-group">
        <input v-model="password" type="password" placeholder="Password" class="form-control">
      </div>
      <button class="btn btn-primary" type="submit">Login</button>
      <p class="text-center mt-3">Or you want login with Google Account</p>
    </form>
  </div>
</template>

<script>
export default {
  name: 'LoginForm',
  data() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    login() {
      let payload = {
        email: this.email,
        password: this.password
      }

      this.$emit("loginPayload", payload)
      // this.$emit("refetchTasks")
    }
  }
}
</script>

<style>

</style>