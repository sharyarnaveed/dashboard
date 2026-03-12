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



      <!-- Social Login Buttons -->
      <div class="space-y-3">
        <button
          type="button"
          @click="handleMicrosoftLogin"
          :disabled="isLoading"
          class="w-full flex items-center justify-center gap-3 rounded-lg border border-border px-4 py-3 text-text hover:bg-surface-light transition disabled:opacity-50 disabled:cursor-not-allowed"
        >
          <UIcon name="i-simple-icons-microsoft" class="h-5 w-5" />
          <span class="text-sm font-medium">Continue with Microsoft</span>
        </button>
        <button
          type="button"
          @click="handleGoogleLogin"
          :disabled="isLoading"
          class="w-full flex items-center justify-center gap-3 rounded-lg border border-border px-4 py-3 text-text hover:bg-surface-light transition disabled:opacity-50 disabled:cursor-not-allowed"
        >
          <UIcon name="i-simple-icons-google" class="h-5 w-5" />
          <span class="text-sm font-medium">Continue with Google</span>
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

const isLoading = ref(false)
const error = ref('')
const success = ref('')

const handleMicrosoftLogin = async () => {
  isLoading.value = true
  try {
    // TODO: Implement Microsoft login
    await new Promise(resolve => setTimeout(resolve, 1000))
    success.value = 'Redirecting to Microsoft login...'
    setTimeout(() => {
      navigateTo('/dashboard')
    }, 1000)
  } catch (err) {
    error.value = 'Failed to connect with Microsoft. Please try again.'
  } finally {
    isLoading.value = false
  }
}

const handleGoogleLogin = async () => {
  isLoading.value = true
  try {
    // TODO: Implement Google login
    await new Promise(resolve => setTimeout(resolve, 1000))
    success.value = 'Redirecting to Google login...'
    setTimeout(() => {
      navigateTo('/dashboard')
    }, 1000)
  } catch (err) {
    error.value = 'Failed to connect with Google. Please try again.'
  } finally {
    isLoading.value = false
  }
}
</script>
