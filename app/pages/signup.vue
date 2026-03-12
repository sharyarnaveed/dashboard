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
        <h1 class="text-2xl font-bold">Create Your Account</h1>
        <p class="mt-2 text-text-secondary">Start scheduling your classes with ClassSync</p>
      </div>

      <!-- Signup Form -->
      <form @submit.prevent="handleSignup" class="space-y-4">
        <!-- Name Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Full Name</label>
          <input
            v-model="fullName"
            type="text"
            placeholder="John Doe"
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
          />
        </div>

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

        <!-- Confirm Password Input -->
        <div>
          <label class="block text-sm font-medium mb-2">Confirm Password</label>
          <input
            v-model="confirmPassword"
            type="password"
            placeholder="••••••••"
            class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
          />
        </div>

        <!-- Terms Checkbox -->
        <label class="flex items-start gap-2">
          <input v-model="agreeTerms" type="checkbox" class="rounded border border-border mt-1" />
          <span class="text-sm text-text-secondary">
            I agree to the
            <a href="#" class="text-primary-400 hover:text-primary-300">Terms of Service</a>
            and
            <a href="#" class="text-primary-400 hover:text-primary-300">Privacy Policy</a>
          </span>
        </label>

        <!-- Signup Button -->
        <button
          type="submit"
          class="w-full mt-6 rounded-lg bg-primary-600 px-4 py-2 font-semibold text-white hover:bg-primary-700 transition"
        >
          Create Account
        </button>
      </form>

      <!-- Divider -->
      <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
          <div class="w-full border-t border-border"></div>
        </div>
        <div class="relative flex justify-center text-sm">
          <span class="px-2 bg-background text-text-secondary">Already have an account?</span>
        </div>
      </div>

      <!-- Login Link -->
      <NuxtLink
        to="/login"
        class="block w-full text-center rounded-lg border border-border px-4 py-2 font-semibold text-text hover:bg-surface-light transition"
      >
        Sign In
      </NuxtLink>

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

const fullName = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const agreeTerms = ref(false)

const handleSignup = () => {
  navigateTo('/dashboard')
}
</script>
