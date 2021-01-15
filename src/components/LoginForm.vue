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
      <g-signin-button
        :params="googleSignInParams"
        @success="onSignInSuccess"
        @error="onSignInError">
        Sign in with Google
      </g-signin-button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'LoginForm',
  data() {
    return {
      email: '',
      password: '',
      googleSignInParams: {
        client_id: '829416258802-stdqvfut3cunfbfs875oifuhsr5iq6gf.apps.googleusercontent.com'
      }
    }
  },
  methods: {
    onSignInSuccess (googleUser) {
      // `googleUser` is the GoogleUser object that represents the just-signed-in user.
      // See https://developers.google.com/identity/sign-in/web/reference#users
      const id_token = googleUser.getAuthResponse().id_token; // etc etc
      this.$emit("pasIdToken", id_token)
      // axios({
      //   method: 'post',
      //   url: 'http://localhost:3000/googleLogin',
      //   data: {id_token}
      // })
      // .then((response) => {
      //   console.log("RESPON THEN GOOGLE LOGIN", response);
      //   localStorage.setItem("access_token", response.data.access_token)
      //   console.log(response);
      //   this.$emit("currentPage", 'home')
      //   this.$emit("refectchTasks")
      // })
      // .catch((error) => {
      //   console.log("ERROR LOGIN GOOGLE", error);
      //   this.onSignInError(error)
      // })
    },

    onSignInError (error) {
      // `error` contains any error occurred.
      console.log('OH NOES', error)
    },

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
  .g-signin-button {
  /* This is where you control how the button looks. Be creative! */
  display: inline-block;
  padding: 4px 8px;
  border-radius: 3px;
  background-color: #3c82f7;
  color: #fff;
  box-shadow: 0 3px 0 #0f69ff;
}
</style>