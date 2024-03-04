<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'
import { authorizationUser } from '../services/userService.js'

const router = useRouter()
const store = useStore()

const email = ref('')
const password = ref('')
const errorMessage = ref('')

async function onLoginSubmit() {
  try {
    const user = await authorizationUser(email.value, password.value)
    store.dispatch('login', user.username)
    router.push({ name: 'Home' })
  } catch (ex) {
    const {
      response: { data }
    } = ex
    errorMessage.value = data
  }
}
</script>

<template>
  <form
    class="w-1/3 max-lg:w-1/2 max-sm:w-10/12 bg-white shadow-xl border px-4 pt-10 pb-7 rounded-3xl fixed top-1/4 left-1/2 -translate-x-1/2"
    @submit.prevent="onLoginSubmit"
  >
    <h2 class="font-sans text-gray-700 font-bold pb-4 text-center text-3xl">
      Авторизация
    </h2>

    <div class="flex flex-col gap-3 mt-4">
      <p
        v-if="errorMessage"
        class="text-center font-bold bg-red-600 text-white py-2 rounded-3xl select-none cursor-pointer transition hover:bg-red-700"
        @click="() => (errorMessage = '')"
      >
        {{ errorMessage }}
      </p>
      <input
        required
        class="font-sans shadow-xl bg-gray-100 py-3 mb-3 px-3 outline-none rounded-3xl transition placeholder:italic placeholder-gray-900 text-gray-900"
        type="email"
        placeholder="Введите email..."
        v-model="email"
      />
      <input
        required
        minlength="4"
        class="font-sans shadow-xl bg-gray-100 py-3 mb-3 px-3 outline-none rounded-3xl transition placeholder:italic placeholder-gray-700 text-gray-700"
        type="password"
        placeholder="Введите пароль..."
        v-model="password"
      />
    </div>

    <div class="text-center">
      <input
        class="shadow-xl font-bold px-10 font-sans bg-gray-100 py-3 mb-3 mt-3 px-6 outline-none rounded-3xl transition placeholder:italic placeholder-gray-700 text-gray-700 cursor-pointer text-center"
        type="submit"
        value="Войти"
      />
    </div>

    <div class="text-center">
      <router-link to="/registration" class="font-sans text-gray-700 hover:underline">Нет аккаунта?</router-link>
    </div>

  </form>
</template>

<style>
body {
  background: rgb(254,232,255);
background: linear-gradient(113deg, rgba(254,232,255,1) 5%, rgba(255,255,255,1) 40%, rgba(255,255,255,1) 51%, rgba(255,255,255,1) 62%, rgba(233,255,254,1) 99%);
    height: 100%;
    margin: 0;
    background-repeat: no-repeat;
    background-attachment: fixed;
}
</style>