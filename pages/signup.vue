<template>
  <div
    class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="max-w-md w-full space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          Sign up for an account
        </h2>
        <p class="mt-2 text-center text-sm text-gray-600">
          Or
          {{ ' ' }}
          <NuxtLink
            to="/login"
            class="font-medium text-indigo-600 hover:text-indigo-500"
          >
            log into your account
          </NuxtLink>
        </p>
      </div>
      <form class="space-y-6" method="post" @submit="signup">
        <div>
          <label for="name" class="block text-sm font-medium text-gray-700">
            Name
          </label>
          <div class="mt-1">
            <input
              id="name"
              v-model="name"
              name="name"
              type="name"
              autocomplete="name"
              required
              :class="{ 'border-red-200': nameErrors }"
              class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            />
            <FieldErrors v-if="nameErrors" :field-errors="nameErrors" />
          </div>
        </div>
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">
            Email address
          </label>
          <div class="mt-1">
            <input
              id="email"
              v-model="email"
              name="email"
              type="email"
              autocomplete="email"
              required
              :class="{ 'border-red-200': emailErrors }"
              class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            />
            <FieldErrors v-if="emailErrors" :field-errors="emailErrors" />
          </div>
        </div>

        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">
            Password
          </label>
          <div class="mt-1">
            <input
              id="password"
              v-model="password"
              name="password"
              type="password"
              autocomplete="new-password"
              required
              :class="{ 'border-red-200': passwordErrors }"
              class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            />
            <FieldErrors v-if="passwordErrors" :field-errors="passwordErrors" />
          </div>
        </div>

        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input
              id="remember-me"
              v-model="rememberMe"
              name="remember-me"
              type="checkbox"
              class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
            />
            <label for="remember-me" class="ml-2 block text-sm text-gray-900">
              Remember me
            </label>
          </div>
        </div>

        <div>
          <button
            type="submit"
            class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            Sign up
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import FieldErrors from '~/components/FieldErrors.vue'
export default {
  name: 'SignupPage',
  auth: 'guest',
  components: {
    FieldErrors,
  },
  data() {
    return {
      name: null,
      email: null,
      password: null,
      rememberMe: false,
      errors: null,
    }
  },
  computed: {
    nameErrors() {
      return this.errors?.name
    },
    emailErrors() {
      return this.errors?.email
    },
    passwordErrors() {
      return this.errors?.password
    },
  },
  methods: {
    signup(e) {
      e.preventDefault()
      this.errors = null
      if (!this.email | !this.password | !this.name) {
        this.errors = {
          name: 'Name is required',
          email: 'Email is required',
          password: 'Password is required',
        }
        return
      }
      const payload = {
        name: this.name,
        email: this.email,
        password: this.password,
        rememberMe: this.rememberMe,
      }
      this.$axios.get('/sanctum/csrf-cookie').then(() => {
        this.$axios
          .post('/api/signup', payload)
          .then((response) => {
            if (response.status === 201) {
              this.$auth.loginWith('laravelSanctum', {
                data: {
                  email: this.email,
                  password: this.password,
                  rememberMe: this.rememberMe,
                },
              })
              this.$router.push('/dashboard')
            }
          })
          .catch((e) => {
            if (e.response.status === 422) {
              this.errors = e.response.data.errors
            }
          })
      })
    },
  },
}
</script>
