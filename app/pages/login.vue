<template>
  <div class="min-h-screen bg-background text-text flex items-center justify-center px-4 py-12">
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

      <!-- Error Alert -->
      <div v-if="error" class="mb-4 rounded-lg bg-error bg-opacity-10 border border-error border-opacity-20 p-4 text-error text-sm flex items-start gap-3">
        <UIcon name="i-lucide-alert-circle" class="h-5 w-5 flex-shrink-0 mt-0.5" />
        <div>{{ error }}</div>
      </div>

      <!-- Success Alert -->
      <div v-if="success" class="mb-4 rounded-lg bg-success bg-opacity-10 border border-success border-opacity-20 p-4 text-success text-sm flex items-start gap-3">
        <UIcon name="i-lucide-check-circle" class="h-5 w-5 flex-shrink-0 mt-0.5" />
        <div>{{ success }}</div>
      </div>

      <!-- Login Form -->
      <form @submit.prevent="handleLogin" class="space-y-4">
        <!-- Email Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Email Address</label>
          <input
            v-model="formData.email"
            type="email"
            placeholder="john@school.edu"
            :disabled="isLoading"
            required
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-3 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition disabled:opacity-50 disabled:cursor-not-allowed"
          />
          <p v-if="fieldErrors.email" class="mt-1 text-xs text-error flex items-center gap-1">
            <UIcon name="i-lucide-alert-circle" class="h-3 w-3" />
            {{ fieldErrors.email }}
          </p>
        </div>

        <!-- Password Input -->
        <div>
          <div class="flex items-center justify-between mb-2">
            <label class="block text-sm font-medium">Password</label>
            <NuxtLink to="/forgot-password" class="text-xs text-primary-400 hover:text-primary-300 transition">
              Forgot password?
            </NuxtLink>
          </div>
          <input
            v-model="formData.password"
            type="password"
            placeholder="••••••••"
            :disabled="isLoading"
            required
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-3 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition disabled:opacity-50 disabled:cursor-not-allowed"
          />
          <p v-if="fieldErrors.password" class="mt-1 text-xs text-error flex items-center gap-1">
            <UIcon name="i-lucide-alert-circle" class="h-3 w-3" />
            {{ fieldErrors.password }}
          </p>
        </div>

        <!-- Remember Me -->
        <label class="flex items-center gap-2 cursor-pointer">
          <input v-model="formData.rememberMe" type="checkbox" :disabled="isLoading" class="rounded border border-border cursor-pointer" />
          <span class="text-sm text-text-secondary">Remember me for 30 days</span>
        </label>

        <!-- Login Button -->
        <button
          type="submit"
          :disabled="isLoading"
          class="w-full mt-6 rounded-lg bg-primary-600 hover:bg-primary-700 disabled:bg-primary-600 disabled:opacity-50 px-4 py-3 font-semibold text-white transition flex items-center justify-center gap-2"
        >
          <span v-if="isLoading" class="inline-block h-4 w-4 animate-spin rounded-full border-2 border-white border-r-transparent"></span>
          {{ isLoading ? 'Signing in...' : 'Sign In' }}
        </button>
      </form>

      <!-- Divider -->
      <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
          <div class="w-full border-t border-border"></div>
        </div>
        <div class="relative flex justify-center text-sm">
          <span class="px-2 bg-background text-text-secondary">Or continue with</span>
        </div>
      </div>

      <!-- Social Login Buttons -->
      <div class="grid grid-cols-2 gap-3">
        <button
          type="button"
          :disabled="isLoading"
          class="flex items-center justify-center gap-2 rounded-lg border border-border px-4 py-3 text-text hover:bg-surface-light transition disabled:opacity-50 disabled:cursor-not-allowed"
        >
          <UIcon name="i-simple-icons-microsoft" class="h-5 w-5" />
          <span class="hidden sm:inline text-sm">Microsoft</span>
        </button>
        <button
          type="button"
          :disabled="isLoading"
          class="flex items-center justify-center gap-2 rounded-lg border border-border px-4 py-3 text-text hover:bg-surface-light transition disabled:opacity-50 disabled:cursor-not-allowed"
        >
          <UIcon name="i-simple-icons-google" class="h-5 w-5" />
          <span class="hidden sm:inline text-sm">Google</span>
        </button>
      </div>

      <!-- Signup Link -->
      <p class="mt-6 text-center text-sm text-text-secondary">
        Don't have an account?
        <NuxtLink to="/signup" class="text-primary-400 hover:text-primary-300 font-medium transition">
          Create one
        </NuxtLink>
      </p>

      <!-- Footer -->
      <p class="mt-6 text-center text-xs text-text-secondary">
        By signing in, you agree to our
        <a href="#" class="text-primary-400 hover:text-primary-300 transition">Terms of Service</a>
        and
        <a href="#" class="text-primary-400 hover:text-primary-300 transition">Privacy Policy</a>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: false
})

const formData = ref({
  email: '',
  password: '',
  rememberMe: false
})

const isLoading = ref(false)
const error = ref('')
const success = ref('')

const fieldErrors = ref({
  email: '',
  password: ''
})

const validateForm = (): boolean => {
  fieldErrors.value = { email: '', password: '' }
  let isValid = true

  if (!formData.value.email.trim()) {
    fieldErrors.value.email = 'Email is required'
    isValid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.value.email)) {
    fieldErrors.value.email = 'Please enter a valid email address'
    isValid = false
  }

  if (!formData.value.password) {
    fieldErrors.value.password = 'Password is required'
    isValid = false
  } else if (formData.value.password.length < 6) {
    fieldErrors.value.password = 'Password must be at least 6 characters'
    isValid = false
  }

  return isValid
}

const handleLogin = async () => {
  error.value = ''
  success.value = ''

  if (!validateForm()) {
    return
  }

  isLoading.value = true

  try {
    // Simulate API call delay
    await new Promise(resolve => setTimeout(resolve, 1500))
    
    // Simulate successful login
    success.value = 'Login successful! Redirecting to dashboard...'
    
    // Redirect after brief delay
    setTimeout(() => {
      navigateTo('/dashboard')
    }, 1000)
  } catch (err) {
    error.value = 'Invalid email or password. Please try again.'
  } finally {
    isLoading.value = false
  }
}
</script>
