<template>
  <div class="flex items-center justify-center h-96">
    <div class="bg-white p-8 rounded shadow-inner w-96">
      <h1 class="text-2xl text-center font-semibold mb-4">Login Page</h1>
      <div class="flex flex-col gap-4">
        <input
          type="text"
          required
          v-model="username"
          class="border p-2 rounded"
          placeholder="Username"
        />
        <input
          type="password"
          required
          v-model="password"
          class="border p-2 rounded"
          placeholder="Password"
        />
        <button
          @click="onLogin"
          class="bg-violet-500 text-white py-2 px-4 rounded hover:bg-violet-600"
        >
          Login
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "@/stores/auth";
import axios from "axios";

const auth = useAuthStore();
const username = ref("");
const password = ref("");
const router = useRouter();

const onLogin = async () => {
    try {
      const response = await axios.post('http://localhost:3000/login', {
        username: username.value,
        password: password.value,
      });

      console.log(response)

      if (response.data.status === 'success') {
        auth.login(username.value);
        router.push('/');
      } else {
        window.alert('Username atau password salah');
      }
    } catch (error) {
      console.error('Error during login:', error);
      window.alert('Terjadi kesalahan saat melakukan login');
    }
  };
</script>