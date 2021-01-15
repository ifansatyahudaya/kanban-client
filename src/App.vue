<template>
  <!-- pemanggilan component bsa pke pascal-case/kebab case -->
  <span>
    <Navbar v-bind:currentUser="currentUser"
      v-on:logout="logoutFunction"
      v-bind:currentPage="currentPage">
    </Navbar>

    <div class="container">
      <div class="row justify-content-md-center">

        <div class="col-md-auto">
          <ul class="form-login">
            <li v-if="currentPage == 'login' || currentPage == 'register'"><button @click="currentPage='login'" class="btn btn-link">Login</button></li>
            <li v-if="currentPage == 'login' || currentPage == 'register'"><button @click="currentPage='register'" class="btn btn-link">Register</button></li>
          </ul>
          <LoginForm v-if="currentPage == 'login'"
            v-on:loginPayload="loginFunction"
            v-on:refectchTasks="fetchAllTask">

          </LoginForm>

          <RegisterForm v-else-if="currentPage == 'register'"
            v-on:registerPayload="registerFunction">
          </RegisterForm>

          <Home v-else v-bind:tasks="tasks"
            v-bind:currentUser="currentUser"
            v-on:addTaskPayload="addTask"
            v-on:onDelete="deleteTask"
            v-on:onUpdateTask="onUpdateTask"
            v-on:onMoveCategory="onMoveCategory">
          </Home>
        </div>
      </div>
    </div>

  </span>
</template>

<script>
import axios from 'axios'
import LoginForm from "./components/LoginForm"
import Home from "./components/Home"
import Navbar from './components/Navbar'
import RegisterForm from "./components/RegisterForm"


export default {
  name: 'App',
  data() {
    return {
      message: 'Hello from my kamvang',
      currentPage: 'login',
      tasks: [],
      baseUrl: 'http://localhost:3000',
      currentUser: JSON.parse(localStorage.getItem('currentUser') || "{}")
    }
  },
  components: {
    LoginForm,
    Home,
    Navbar,
    RegisterForm
  },
  methods: {
    // registerFunction
    registerFunction(payload) {
      axios({
        method: 'post',
        url: `${this.baseUrl}/register`,
        data: {
          email: payload.email,
          password: payload.password,
          name: payload.name
        }
      })
      .then(() => {
        this.currentPage = 'login'
      })
      .catch(() => {
        console.log(error);
      })
    },

    // loginFunction
    loginFunction(payload) {
      axios({
        method: 'post',
        url: `${this.baseUrl}/login`,
        data: {
          email: payload.email,
          password: payload.password
        }
      })
      .then((response) => {
        // console.log('YESSSS', response.data);
        localStorage.setItem('access_token', response.data.access_token)
        localStorage.setItem('currentUser', JSON.stringify(response.data._user))
        this.currentUser = response.data._user
        this.currentPage = 'home'
        this.fetchAllTask()
      })
      .catch((error) => {
        console.log(error);
      })
    },

    // logout
    logoutFunction() {
      localStorage.clear()
      this.currentUser = {}
      this.currentPage = 'login'
    },

    // findAll tasks
    fetchAllTask() {
      axios({
        method: 'get',
        url: `${this.baseUrl}/tasks`,
        headers: {
          access_token: localStorage.access_token
        }
      })
      .then((response) => {
        console.log(response.data);
        this.tasks = response.data

      })
      .catch((error) => {
        console.log(error);
      })

    },

    // add tasks
    addTask(payload) {
      axios({
        method: 'post',
        url: `${this.baseUrl}/tasks`,
         headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: payload.name,
          CategoryId: payload.CategoryId,
          UserId: this.currentUser.id
        }
      })
      .then((response) => {
        this.fetchAllTask()
        payload.clearName()
      })
      .catch((error) => {
        console.log(error);
      })
    },

    // findOne
    findOne() {

    },

    // Update Task Name
    onUpdateTask(payload, callback) {
      axios({
        method: 'put',
        url: `${this.baseUrl}/tasks/${payload.id}`,
         headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: payload.name,
        }
      })
      .then((response) => {
        this.fetchAllTask()
        callback()
      })
      .catch((error) => {
        console.log(error);
      })
    },

    // Set Status by CategoryId
    setStatus() {

    },

    // delete task
    deleteTask(id) {
      // console.log(id,"ASASASASASAS");
      axios({
        method: 'delete',
        url: `${this.baseUrl}/tasks/${id}`,
        headers: {
          // access_token: localStorage.getItem("access_token")
          access_token: localStorage.access_token
        }
      })
      .then(() => {
        this.fetchAllTask()
      })
      .catch((error) => {
        console.log(error);
      }).finally(() => {
        // console.log('asdfasdf')
      })
    },

    onMoveCategory(payload) {
      axios({
        method: 'patch',
        url: `${this.baseUrl}/tasks/${payload.id}`,
         headers: {
          access_token: localStorage.access_token
        },
        data: {
          CategoryId: payload.CategoryId,
        }
      })
      .then((response) => {
        this.fetchAllTask()
      })
      .catch((error) => {
        console.log(error);
      })
    }
  },
  created() {
    if (localStorage.getItem("access_token")) {
      this.currentPage = 'home'
      this.fetchAllTask()
    }
  }

}
</script>

<style>

</style>