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
        <h1 class="text-2xl font-bold">Create Your Account</h1>
        <p class="mt-2 text-text-secondary">Start scheduling your classes with ClassSync</p>
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

      <!-- Signup Form -->
      <form @submit.prevent="handleSignup" class="space-y-4">
        <!-- Full Name Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Full Name</label>
          <input
            v-model="formData.fullName"
            type="text"
            placeholder="John Doe"
            :disabled="isLoading"
            required
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-3 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition disabled:opacity-50 disabled:cursor-not-allowed"
          />
          <p v-if="fieldErrors.fullName" class="mt-1 text-xs text-error flex items-center gap-1">
            <UIcon name="i-lucide-alert-circle" class="h-3 w-3" />
            {{ fieldErrors.fullName }}
          </p>
        </div>

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
          <label class="block text-sm font-medium mb-2">Password</label>
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
          <p class="mt-1 text-xs text-text-secondary">At least 8 characters with uppercase, lowercase, and numbers</p>
        </div>

        <!-- Confirm Password Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Confirm Password</label>
          <input
            v-model="formData.confirmPassword"
            type="password"
            placeholder="••••••••"
            :disabled="isLoading"
            required
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-3 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition disabled:opacity-50 disabled:cursor-not-allowed"
          />
          <p v-if="fieldErrors.confirmPassword" class="mt-1 text-xs text-error flex items-center gap-1">
            <UIcon name="i-lucide-alert-circle" class="h-3 w-3" />
            {{ fieldErrors.confirmPassword }}
          </p>
        </div>

        <!-- Terms Checkbox -->
        <label class="flex items-start gap-3 cursor-pointer">
          <input v-model="formData.agreeTerms" type="checkbox" :disabled="isLoading" class="rounded border border-border mt-1 cursor-pointer" />
          <span class="text-sm text-text-secondary">
            I agree to the
            <a href="#" class="text-primary-400 hover:text-primary-300 transition">Terms of Service</a>
            and
            <a href="#" class="text-primary-400 hover:text-primary-300 transition">Privacy Policy</a>
          </span>
        </label>
        <p v-if="fieldErrors.agreeTerms" class="text-xs text-error flex items-center gap-1">
          <UIcon name="i-lucide-alert-circle" class="h-3 w-3" />
          {{ fieldErrors.agreeTerms }}
        </p>

        <!-- Signup Button -->
        <button
          type="submit"
          :disabled="isLoading"
          class="w-full mt-6 rounded-lg bg-primary-600 hover:bg-primary-700 disabled:bg-primary-600 disabled:opacity-50 px-4 py-3 font-semibold text-white transition flex items-center justify-center gap-2"
        >
          <span v-if="isLoading" class="inline-block h-4 w-4 animate-spin rounded-full border-2 border-white border-r-transparent"></span>
          {{ isLoading ? 'Creating account...' : 'Create Account' }}
        </button>
      </form>

      <!-- Divider -->
      <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
          <div class="w-full border-t border-border"></div>
        </div>
        <div class="relative flex justify-center text-sm">
          <span class="px-2 bg-background text-text-secondary">Or sign up with</span>
        </div>
      </div>

      <!-- Social Signup -->
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

      <!-- Login Link -->
      <p class="mt-6 text-center text-sm text-text-secondary">
        Already have an account?
        <NuxtLink to="/login" class="text-primary-400 hover:text-primary-300 font-medium transition">
          Sign in
        </NuxtLink>
      </p>

      <!-- Footer -->
      <p class="mt-6 text-center text-xs text-text-secondary">
        Start your free account today. No credit card required.
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: false
})

const formData = ref({
  fullName: '',
  email: '',
  password: '',
  confirmPassword: '',
  agreeTerms: false
})

const isLoading = ref(false)
const error = ref('')
const success = ref('')

const fieldErrors = ref({
  fullName: '',
  email: '',
  password: '',
  confirmPassword: '',
  agreeTerms: ''
})

const validatePassword = (password: string): boolean => {
  // At least 8 characters, uppercase, lowercase, and number
  const hasMinLength = password.length >= 8
  const hasUppercase = /[A-Z]/.test(password)
  const hasLowercase = /[a-z]/.test(password)
  const hasNumber = /[0-9]/.test(password)
  return hasMinLength && hasUppercase && hasLowercase && hasNumber
}

const validateForm = (): boolean => {
  fieldErrors.value = {
    fullName: '',
    email: '',
    password: '',
    confirmPassword: '',
    agreeTerms: ''
  }
  let isValid = true

  if (!formData.value.fullName.trim()) {
    fieldErrors.value.fullName = 'Full name is required'
    isValid = false
  }

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
  } else if (!validatePassword(formData.value.password)) {
    fieldErrors.value.password = 'Password must be 8+ characters with uppercase, lowercase, and numbers'
    isValid = false
  }

  if (formData.value.password !== formData.value.confirmPassword) {
    fieldErrors.value.confirmPassword = 'Passwords do not match'
    isValid = false
  }

  if (!formData.value.agreeTerms) {
    fieldErrors.value.agreeTerms = 'You must agree to the terms'
    isValid = false
  }

  return isValid
}

const handleSignup = async () => {
  error.value = ''
  success.value = ''

  if (!validateForm()) {
    return
  }

  isLoading.value = true

  try {
    // Simulate API call delay
    await new Promise(resolve => setTimeout(resolve, 1500))
    
    // Simulate successful signup
    success.value = 'Account created successfully! Redirecting to dashboard...'
    
    // Redirect after brief delay
    setTimeout(() => {
      navigateTo('/dashboard')
    }, 1000)
  } catch (err) {
    error.value = 'Failed to create account. Please try again.'
  } finally {
    isLoading.value = false
  }
}
</script>
