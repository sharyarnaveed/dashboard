<template>
  <div class="min-h-screen bg-background text-text flex items-center justify-center px-4">
    <div class="w-full max-w-md">
      <!-- Header -->
      <div class="text-center mb-8">
        <div class="flex justify-center mb-4">
          <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600">
            <span class="text-lg font-bold text-white">T</span>
          </div>
        </div>
        <h1 class="text-2xl font-bold">Welcome Back</h1>
        <p class="mt-2 text-text-secondary">Sign in to your ClassSync account</p>
      </div>

      <!-- Login Form -->
      <form @submit.prevent="handleLogin" class="space-y-4">
        <!-- Email Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Email Address</label>
          <input
            v-model="email"
            type="email"
            placeholder="john@school.edu"
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
          />
        </div>

        <!-- Password Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Password</label>
          <input
            v-model="password"
            type="password"
            placeholder="••••••••"
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
          />
        </div>

        <!-- Remember Me & Forgot Password -->
        <div class="flex items-center justify-between">
          <label class="flex items-center gap-2">
            <input v-model="rememberMe" type="checkbox" class="rounded border border-border" />
            <span class="text-sm text-text-secondary">Remember me</span>
          </label>
          <NuxtLink to="/forgot-password" class="text-sm text-primary-400 hover:text-primary-300">
            Forgot password?
          </NuxtLink>
        </div>

        <!-- Login Button -->
        <button
          type="submit"
          class="w-full mt-6 rounded-lg bg-primary-600 px-4 py-2 font-semibold text-white hover:bg-primary-700 transition"
        >
          Sign In
        </button>
      </form>

      <!-- Divider -->
      <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
          <div class="w-full border-t border-border"></div>
        </div>
        <div class="relative flex justify-center text-sm">
          <span class="px-2 bg-background text-text-secondary">Don't have an account?</span>
        </div>
      </div>

      <!-- Signup Link -->
      <NuxtLink
        to="/signup"
        class="block w-full text-center rounded-lg border border-border px-4 py-2 font-semibold text-text hover:bg-surface-light transition"
      >
        Create Account
      </NuxtLink>

      <!-- Footer -->
      <p class="mt-6 text-center text-xs text-text-secondary">
        By signing in, you agree to our
        <a href="#" class="text-primary-400 hover:text-primary-300">Terms of Service</a>
        and
        <a href="#" class="text-primary-400 hover:text-primary-300">Privacy Policy</a>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: false
})

const email = ref('')
const password = ref('')
const rememberMe = ref(false)

const handleLogin = () => {
  navigateTo('/dashboard')
}
</script>
