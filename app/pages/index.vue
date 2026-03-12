<template>
  <div class="min-h-screen bg-background text-text">
    <!-- Navigation Header -->
    <header class="border-b border-border bg-surface sticky top-0 z-50">
      <div class="mx-auto max-w-7xl px-4 py-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="flex h-8 w-8 items-center justify-center rounded-lg bg-primary-600">
              <span class="text-sm font-bold text-white">T</span>
            </div>
            <span class="text-lg font-semibold">ClassSync</span>
          </div>
          <nav class="flex gap-6">
            <NuxtLink to="/login" class="text-text-secondary hover:text-text transition">Sign In</NuxtLink>
            <NuxtLink to="/signup" class="rounded-lg bg-primary-600 px-4 py-2 font-medium text-white hover:bg-primary-700 transition">Get Started</NuxtLink>
          </nav>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section class="border-b border-border px-4 py-20 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-3xl text-center">
        <h1 class="text-balance text-5xl font-bold tracking-tight sm:text-6xl">
          Automatically schedule your online classes in seconds
        </h1>
        <p class="mt-6 text-xl text-text-secondary">
          Upload your timetable once and ClassSync automatically generates meeting links for Zoom and Google Meet. Save hours on scheduling and focus on teaching.
        </p>
        <div class="mt-10 flex gap-4 justify-center">
          <NuxtLink to="/signup" class="rounded-lg bg-primary-600 px-6 py-3 font-semibold text-white hover:bg-primary-700 transition">
            Get Started Free
          </NuxtLink>
          <a href="#features" class="rounded-lg border border-border px-6 py-3 font-semibold text-text hover:bg-surface-light transition">
            Learn More
          </a>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section id="features" class="border-b border-border px-4 py-20 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-7xl">
        <div class="text-center">
          <h2 class="text-3xl font-bold">Why Teachers Love ClassSync</h2>
          <p class="mt-4 text-text-secondary">Everything you need to manage your online classes efficiently</p>
        </div>

        <div class="mt-16 grid gap-8 sm:grid-cols-2 lg:grid-cols-3">
          <!-- Feature 1 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-zap" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">Auto-Generate Links</h3>
            <p class="mt-2 text-text-secondary">Automatically create Zoom and Google Meet links for all your classes in seconds</p>
          </div>

          <!-- Feature 2 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-calendar" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">Smart Calendar</h3>
            <p class="mt-2 text-text-secondary">View all your classes in a beautiful weekly calendar view with platform icons</p>
          </div>

          <!-- Feature 3 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-upload" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">Easy Upload</h3>
            <p class="mt-2 text-text-secondary">Simply drag and drop your Excel timetable to get started instantly</p>
          </div>

          <!-- Feature 4 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-link" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">One-Click Sharing</h3>
            <p class="mt-2 text-text-secondary">Copy and share meeting links with students instantly</p>
          </div>

          <!-- Feature 5 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-shield" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">Secure & Private</h3>
            <p class="mt-2 text-text-secondary">Your data is encrypted and secure with enterprise-grade protection</p>
          </div>

          <!-- Feature 6 -->
          <div class="rounded-lg border border-border bg-surface p-8 hover:bg-surface-light transition">
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-check-circle" class="h-6 w-6 text-primary-400" />
            </div>
            <h3 class="mt-4 text-lg font-semibold">Zero Setup</h3>
            <p class="mt-2 text-text-secondary">No complicated configurations, just connect and you're ready to go</p>
          </div>
        </div>
      </div>
    </section>

    <!-- How It Works -->
    <section class="border-b border-border px-4 py-20 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-5xl">
        <div class="text-center">
          <h2 class="text-3xl font-bold">How It Works</h2>
          <p class="mt-4 text-text-secondary">Get started in three simple steps</p>
        </div>

        <div class="mt-16 space-y-8">
          <!-- Step 1 -->
          <div class="flex gap-6">
            <div class="flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-primary-600">
              <span class="text-lg font-bold text-white">1</span>
            </div>
            <div>
              <h3 class="text-xl font-semibold">Upload Your Timetable</h3>
              <p class="mt-2 text-text-secondary">Upload your Excel timetable with course names, days, and times</p>
            </div>
          </div>

          <!-- Step 2 -->
          <div class="flex gap-6">
            <div class="flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-primary-600">
              <span class="text-lg font-bold text-white">2</span>
            </div>
            <div>
              <h3 class="text-xl font-semibold">Select Your Platforms</h3>
              <p class="mt-2 text-text-secondary">Choose Zoom or Google Meet for each class (or let us decide)</p>
            </div>
          </div>

          <!-- Step 3 -->
          <div class="flex gap-6">
            <div class="flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-primary-600">
              <span class="text-lg font-bold text-white">3</span>
            </div>
            <div>
              <h3 class="text-xl font-semibold">Share With Students</h3>
              <p class="mt-2 text-text-secondary">Meeting links are automatically generated and ready to share</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Platforms Supported -->
    <section class="border-b border-border px-4 py-20 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-7xl">
        <div class="text-center">
          <h2 class="text-3xl font-bold">Platforms Supported</h2>
          <p class="mt-4 text-text-secondary">Works seamlessly with the most popular video conferencing tools</p>
        </div>

        <div class="mt-16 flex justify-center gap-12">
          <div class="text-center">
            <div class="mx-auto h-16 w-16 rounded-lg bg-surface border border-border p-3">
              <UIcon name="i-lucide-video" class="h-full w-full text-primary-400" />
            </div>
            <h3 class="mt-4 font-semibold">Zoom</h3>
          </div>
          <div class="text-center">
            <div class="mx-auto h-16 w-16 rounded-lg bg-surface border border-border p-3">
              <UIcon name="i-lucide-phone" class="h-full w-full text-primary-400" />
            </div>
            <h3 class="mt-4 font-semibold">Google Meet</h3>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="border-b border-border px-4 py-20 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-2xl text-center">
        <h2 class="text-3xl font-bold">Start Scheduling Smarter Today</h2>
        <p class="mt-4 text-text-secondary">Join hundreds of teachers saving hours every week with ClassSync</p>
        <NuxtLink to="/signup" class="mt-8 inline-block rounded-lg bg-primary-600 px-8 py-4 font-semibold text-white hover:bg-primary-700 transition">
          Get Started Free
        </NuxtLink>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-surface border-t border-border px-4 py-12 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-7xl text-center">
        <div class="flex items-center justify-center gap-2 mb-4">
          <div class="flex h-8 w-8 items-center justify-center rounded-lg bg-primary-600">
            <span class="text-sm font-bold text-white">T</span>
          </div>
          <span class="font-semibold">ClassSync</span>
        </div>
        <p class="text-text-secondary">© 2024 ClassSync. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: false
})
</script>
