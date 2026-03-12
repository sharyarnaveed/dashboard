<template>
  <UDashboardPanel id="upload-timetable">
    <template #header>
      <UDashboardNavbar title="Upload Timetable" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>
      </UDashboardNavbar>
    </template>

    <template #body>
      <div class="max-w-4xl">
        <!-- Step 1: Upload File -->
        <div v-if="step === 1" class="space-y-6">
          <div>
            <h2 class="text-2xl font-bold mb-2">Upload Your Timetable</h2>
            <p class="text-text-secondary">Upload an Excel file with your class schedule</p>
          </div>

          <!-- File Upload Area -->
          <div
            @dragover.prevent="isDragOver = true"
            @dragleave.prevent="isDragOver = false"
            @drop.prevent="handleFileDrop"
            class="rounded-lg border-2 border-dashed transition"
            :class="isDragOver ? 'border-primary-500 bg-primary-500 bg-opacity-10' : 'border-border bg-surface'"
          >
            <div class="flex flex-col items-center justify-center gap-4 py-16 px-6">
              <div class="flex h-16 w-16 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                <UIcon name="i-lucide-upload-cloud" class="h-8 w-8 text-primary-400" />
              </div>
              <div class="text-center">
                <p class="font-medium text-text">Drag and drop your Excel file here</p>
                <p class="text-sm text-text-secondary mt-1">or click to browse your computer</p>
              </div>
              <input
                ref="fileInput"
                type="file"
                accept=".xlsx,.xls,.csv"
                @change="handleFileSelect"
                class="hidden"
              />
              <button
                @click="$refs.fileInput.click()"
                class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition"
              >
                Choose File
              </button>
              <p class="text-xs text-text-secondary">Supports: .xlsx, .xls, .csv</p>
            </div>
          </div>

          <!-- Platform Selection -->
          <div v-if="uploadedFile" class="space-y-4">
            <div>
              <h3 class="text-lg font-semibold mb-4">Select Default Meeting Platform</h3>
              <div class="grid gap-4 sm:grid-cols-2">
                <label class="rounded-lg border transition cursor-pointer p-4" :class="selectedPlatform === 'zoom' ? 'border-primary-500 bg-primary-500 bg-opacity-10' : 'border-border hover:bg-surface-light'">
                  <input v-model="selectedPlatform" type="radio" value="zoom" class="hidden" />
                  <div class="flex items-center gap-3">
                    <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                      <UIcon name="i-lucide-video" class="h-6 w-6 text-primary-400" />
                    </div>
                    <div>
                      <p class="font-medium text-text">Zoom</p>
                      <p class="text-xs text-text-secondary">Video conferencing</p>
                    </div>
                  </div>
                </label>

                <label class="rounded-lg border transition cursor-pointer p-4" :class="selectedPlatform === 'google' ? 'border-primary-500 bg-primary-500 bg-opacity-10' : 'border-border hover:bg-surface-light'">
                  <input v-model="selectedPlatform" type="radio" value="google" class="hidden" />
                  <div class="flex items-center gap-3">
                    <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                      <UIcon name="i-lucide-phone" class="h-6 w-6 text-primary-400" />
                    </div>
                    <div>
                      <p class="font-medium text-text">Google Meet</p>
                      <p class="text-xs text-text-secondary">Video meeting</p>
                    </div>
                  </div>
                </label>
              </div>
            </div>

            <div class="flex gap-4 pt-4">
              <button
                @click="step = 2"
                class="flex-1 rounded-lg bg-primary-600 px-6 py-3 font-semibold text-white hover:bg-primary-700 transition"
              >
                Next: Review Classes
              </button>
              <button
                @click="resetUpload"
                class="rounded-lg border border-border px-6 py-3 font-semibold text-text hover:bg-surface-light transition"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>

        <!-- Step 2: Preview -->
        <div v-if="step === 2" class="space-y-6">
          <div>
            <h2 class="text-2xl font-bold mb-2">Review Your Classes</h2>
            <p class="text-text-secondary">Verify your schedule before generating meeting links</p>
          </div>

          <div class="rounded-lg border border-border bg-surface overflow-hidden">
            <div class="overflow-x-auto">
              <table class="w-full">
                <thead class="border-b border-border bg-surface-light">
                  <tr>
                    <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Course</th>
                    <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Day</th>
                    <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Time</th>
                    <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Platform</th>
                  </tr>
                </thead>
                <tbody class="divide-y divide-border">
                  <tr class="hover:bg-surface-light transition">
                    <td class="px-6 py-4 text-text">Mathematics 101</td>
                    <td class="px-6 py-4 text-text">Monday</td>
                    <td class="px-6 py-4 text-text">09:00 AM - 10:30 AM</td>
                    <td class="px-6 py-4"><span class="inline-flex items-center gap-2"><UIcon name="i-lucide-video" class="h-4 w-4 text-primary-400" /><span class="text-sm">{{ selectedPlatform === 'zoom' ? 'Zoom' : 'Google Meet' }}</span></span></td>
                  </tr>
                  <tr class="hover:bg-surface-light transition">
                    <td class="px-6 py-4 text-text">Physics Advanced</td>
                    <td class="px-6 py-4 text-text">Tuesday</td>
                    <td class="px-6 py-4 text-text">11:00 AM - 12:30 PM</td>
                    <td class="px-6 py-4"><span class="inline-flex items-center gap-2"><UIcon name="i-lucide-video" class="h-4 w-4 text-primary-400" /><span class="text-sm">{{ selectedPlatform === 'zoom' ? 'Zoom' : 'Google Meet' }}</span></span></td>
                  </tr>
                  <tr class="hover:bg-surface-light transition">
                    <td class="px-6 py-4 text-text">Chemistry Lab</td>
                    <td class="px-6 py-4 text-text">Wednesday</td>
                    <td class="px-6 py-4 text-text">02:00 PM - 03:30 PM</td>
                    <td class="px-6 py-4"><span class="inline-flex items-center gap-2"><UIcon name="i-lucide-video" class="h-4 w-4 text-primary-400" /><span class="text-sm">{{ selectedPlatform === 'zoom' ? 'Zoom' : 'Google Meet' }}</span></span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <div class="flex gap-4">
            <button
              @click="step = 3"
              class="flex-1 rounded-lg bg-primary-600 px-6 py-3 font-semibold text-white hover:bg-primary-700 transition"
            >
              Generate Meeting Links
            </button>
            <button
              @click="step = 1"
              class="rounded-lg border border-border px-6 py-3 font-semibold text-text hover:bg-surface-light transition"
            >
              Back
            </button>
          </div>
        </div>

        <!-- Step 3: Success -->
        <div v-if="step === 3" class="space-y-6 text-center">
          <div class="flex justify-center">
            <div class="flex h-16 w-16 items-center justify-center rounded-full bg-success bg-opacity-20">
              <UIcon name="i-lucide-check-circle-2" class="h-8 w-8 text-success" />
            </div>
          </div>
          <div>
            <h2 class="text-2xl font-bold">Classes Imported Successfully!</h2>
            <p class="mt-2 text-text-secondary">Your 3 classes have been added with meeting links</p>
          </div>

          <div class="flex gap-4 justify-center pt-4">
            <NuxtLink to="/dashboard" class="rounded-lg bg-primary-600 px-6 py-3 font-semibold text-white hover:bg-primary-700 transition">
              Go to Dashboard
            </NuxtLink>
            <button
              @click="resetUpload"
              class="rounded-lg border border-border px-6 py-3 font-semibold text-text hover:bg-surface-light transition"
            >
              Upload Another File
            </button>
          </div>
        </div>
      </div>
    </template>
  </UDashboardPanel>
</template>

<script setup lang="ts">
const step = ref(1)
const uploadedFile = ref<File | null>(null)
const isDragOver = ref(false)
const selectedPlatform = ref('zoom')
const fileInput = ref()

const handleFileDrop = (event: DragEvent) => {
  isDragOver.value = false
  const files = event.dataTransfer?.files
  if (files && files[0]) {
    uploadedFile.value = files[0]
  }
}

const handleFileSelect = (event: Event) => {
  const input = event.target as HTMLInputElement
  if (input.files && input.files[0]) {
    uploadedFile.value = input.files[0]
  }
}

const resetUpload = () => {
  step.value = 1
  uploadedFile.value = null
  selectedPlatform.value = 'zoom'
}
</script>
