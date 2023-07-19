<script setup>

  import { ref } from 'vue'

  const emit = defineEmits(['close', 'login'])

  const username = ref('kminchelle')
  const password = ref('0lelplR')
  const pwSeen = ref(false)

  const alert = ref('')

  const submit = async (e) => {
    const response = await fetch('https://dummyjson.com/auth/login', {
      headers: {
        'Content-Type': 'application/json'
      },
      method: 'POST',
      body: JSON.stringify({
        username: username.value,
        password: password.value
      })
    })

    const responseStatus = await response.status

    if (responseStatus == 200) {
      const user = ref((await response.json()))

      emit('login', user.value)
      emit('close')
    }

    else if (responseStatus == 400) {
      const responseJson = await response.json()
      alert.value = responseJson.message
    }
  }

</script>

<template>
  <div
    id="auth-modal"
    class="w-100 h-100 position-absolute start-0 top-0"
  >
    
    <div
      id="background"
      @click="$emit('close')"
      class="w-100 h-100 position-absolute start-0 top-0 bg-dark opacity-50"
    ></div>

    <div
      id="container"
      class="bg-white w-50 h-75 mx-auto mt-5 pt-3 text-center rounded"
    >

      <div id="close" class="text-end px-5">
        <button
          @click="$emit('close')"
          class="border-0 bg-transparent cursor-pointer"
        >
          &times;
        </button>
      </div>

      <h2>Login</h2>

      <form @submit.prevent="submit">
        <label class="mt-3">Username :</label>
        <input
          type="text"
          name="username"
          v-model="username"
          class="form-control w-75 mx-auto"
        >
        <label class="mt-3">
          Password :
          <a href="#" @click="pwSeen = ! pwSeen">
            <img :src="'./eye' + (pwSeen ? '' : '_slash') + '.png'" />
          </a>
        </label>
        <input
          :type="pwSeen ? 'text' : 'password'"
          name="password"
          v-model="password"
          class="form-control w-75 mx-auto"
        >
        <input
          type="submit"
          value="Login"
          class="btn btn-primary mt-3"
        >

        <div
          class="alert alert-danger w-75 mx-auto mt-5"
          v-show="alert"
        >
          {{ alert }}
        </div>

      </form>
    </div>

  </div>

</template>

<style scoped>

  #auth-modal {
    z-index: 1;
  }

  #background {
    z-index: -1;
  }
  
  #close {
    font-size: 2em;
  }

  img {
    width: 24px;
    height: auto;
  }

</style>