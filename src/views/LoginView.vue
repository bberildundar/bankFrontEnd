<template>
  <div class="h-screen flex min-h-full flex-1 flex-col justify-center px-6 py-12 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <h2 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">Sign in to your account
      </h2>
    </div>
    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <form class="space-y-6" action="#" method="POST">
        <div>
          <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email address</label>
          <div class="mt-2">

            <input id="email" name="email" type="email" autocomplete="email" required="" placeholder="example@email.com"
              class="px-3 block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-teal-500 sm:text-sm sm:leading-6"
              v-model="username" />
          </div>
        </div>
        <div>
          <div class="flex items-center justify-between">
            <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
          </div>
          <div class="mt-2">
            <input id="password" name="password" type="password" autocomplete="current-password" required=""
              placeholder="***********"
              class="px-3 block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-teal-500 sm:text-sm sm:leading-6"
              v-model="password" />
          </div>
        </div>
        <div>
          <button @click="login" type='button'
            class="flex w-full justify-center rounded-md bg-teal-500 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-teal-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-teal-500">Sign
            in</button>


        </div>
      </form>
      <p class="mt-10 text-center text-sm text-gray-500">
        Not a member?
        {{ ' ' }}
        <router-link to="/register" class="font-semibold leading-6 text-teal-600 hover:text-teal-500"
          active-class="active">Click here to register</router-link>
      </p>

      <div v-if="error" class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
        role="alert">
        <span class="block sm:inline">{{ error }}</span>
        <span class="absolute top-0 bottom-0 right-0 px-4 py-3">
        </span>
      </div>

    </div>
  </div>
</template>

<script >
import { userStore } from '../stores/user.js';
export default {
  name: 'LoginView',
  data() {
    return {
      username: '',
      password: '',
      store: userStore(),
      error: ''
    };
  },
  methods: {
    login() {
      if (this.username.trim() === '' || this.password.trim() === '') {
        this.errorMessage = 'Please enter both username and password.';
        return;
      }
      this.store.login(this.username, this.password)
        .then(
          this.redirctUser()
        )
        .catch((error) => {
          this.handleRequestError(error)
          console.log(error);
        });

    },
    redirctUser() {
      this.store.redirctUser();
    },
    handleRequestError(error) {
      if (error.response) {
        if (error.response.status === 400) {
          this.error = error.response.data.message;
        } else {
          this.error = "An error occurred. Please try again later.";
        }
      }
    }
  }
}
</script>

