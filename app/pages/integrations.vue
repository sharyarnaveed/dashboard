<template>
  <UDashboardPanel id="integrations">
    <template #header>
      <UDashboardNavbar title="Integrations" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>
      </UDashboardNavbar>
    </template>

    <template #body>
      <div class="space-y-8">
        <!-- Zoom Integration -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-start justify-between">
            <div class="flex items-start gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                <UIcon name="i-lucide-video" class="h-6 w-6 text-primary-400" />
              </div>
              <div>
                <h3 class="text-lg font-semibold text-text">Zoom</h3>
                <p class="text-sm text-text-secondary mt-1">Automatically generate Zoom meeting links for your classes</p>

                <div v-if="zoomConnected" class="mt-4 flex items-center gap-2">
                  <div class="h-2 w-2 rounded-full bg-success"></div>
                  <span class="text-sm text-success">Connected</span>
                  <span class="text-xs text-text-secondary">as zoom.user@example.com</span>
                </div>
              </div>
            </div>

            <button
              v-if="!zoomConnected"
              @click="connectZoom"
              class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition"
            >
              Connect
            </button>
            <button
              v-else
              @click="disconnectZoom"
              class="rounded-lg bg-surface-light px-6 py-2 font-medium text-text hover:bg-border transition"
            >
              Disconnect
            </button>
          </div>

          <div v-if="zoomConnected" class="mt-6 border-t border-border pt-4">
            <h4 class="text-sm font-semibold text-text mb-3">Settings</h4>
            <div class="space-y-3">
              <label class="flex items-center gap-3">
                <input v-model="zoomSettings.autoCreate" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Automatically create meeting links</span>
              </label>
              <label class="flex items-center gap-3">
                <input v-model="zoomSettings.requirePassword" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Require password for all meetings</span>
              </label>
            </div>
          </div>
        </div>

        <!-- Google Meet Integration -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-start justify-between">
            <div class="flex items-start gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                <UIcon name="i-lucide-phone" class="h-6 w-6 text-primary-400" />
              </div>
              <div>
                <h3 class="text-lg font-semibold text-text">Google Meet</h3>
                <p class="text-sm text-text-secondary mt-1">Enable Google Meet scheduling for your classes</p>

                <div v-if="googleConnected" class="mt-4 flex items-center gap-2">
                  <div class="h-2 w-2 rounded-full bg-success"></div>
                  <span class="text-sm text-success">Connected</span>
                  <span class="text-xs text-text-secondary">as john@school.edu</span>
                </div>
              </div>
            </div>

            <button
              v-if="!googleConnected"
              @click="connectGoogle"
              class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition"
            >
              Connect
            </button>
            <button
              v-else
              @click="disconnectGoogle"
              class="rounded-lg bg-surface-light px-6 py-2 font-medium text-text hover:bg-border transition"
            >
              Disconnect
            </button>
          </div>

          <div v-if="googleConnected" class="mt-6 border-t border-border pt-4">
            <h4 class="text-sm font-semibold text-text mb-3">Settings</h4>
            <div class="space-y-3">
              <label class="flex items-center gap-3">
                <input v-model="googleSettings.autoCreate" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Automatically create meet links</span>
              </label>
              <label class="flex items-center gap-3">
                <input v-model="googleSettings.addToCalendar" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Add meetings to Google Calendar</span>
              </label>
            </div>
          </div>
        </div>

        <!-- Email Notifications Integration -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-start justify-between">
            <div class="flex items-start gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
                <UIcon name="i-lucide-mail" class="h-6 w-6 text-primary-400" />
              </div>
              <div>
                <h3 class="text-lg font-semibold text-text">Email Notifications</h3>
                <p class="text-sm text-text-secondary mt-1">Receive email updates about your upcoming classes</p>

                <div v-if="emailEnabled" class="mt-4 flex items-center gap-2">
                  <div class="h-2 w-2 rounded-full bg-success"></div>
                  <span class="text-sm text-success">Enabled</span>
                </div>
              </div>
            </div>

            <label class="relative inline-flex cursor-pointer items-center">
              <input v-model="emailEnabled" type="checkbox" class="sr-only peer" />
              <div class="peer relative h-6 w-11 rounded-full bg-surface-light transition peer-checked:bg-primary-600"></div>
              <span class="absolute left-1 top-0.5 h-5 w-5 rounded-full bg-white transition peer-checked:translate-x-5"></span>
            </label>
          </div>

          <div v-if="emailEnabled" class="mt-6 border-t border-border pt-4">
            <h4 class="text-sm font-semibold text-text mb-3">Notification Preferences</h4>
            <div class="space-y-3">
              <label class="flex items-center gap-3">
                <input v-model="emailSettings.classReminder" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Class reminders (15 minutes before)</span>
              </label>
              <label class="flex items-center gap-3">
                <input v-model="emailSettings.newClasses" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">New classes added</span>
              </label>
              <label class="flex items-center gap-3">
                <input v-model="emailSettings.weeklyDigest" type="checkbox" class="rounded border border-border" />
                <span class="text-sm text-text">Weekly schedule digest</span>
              </label>
            </div>
          </div>
        </div>
      </div>
    </template>
  </UDashboardPanel>
</template>

<script setup lang="ts">
const zoomConnected = ref(false)
const googleConnected = ref(false)
const emailEnabled = ref(true)

const zoomSettings = ref({
  autoCreate: true,
  requirePassword: false
})

const googleSettings = ref({
  autoCreate: true,
  addToCalendar: true
})

const emailSettings = ref({
  classReminder: true,
  newClasses: true,
  weeklyDigest: true
})

const connectZoom = () => {
  zoomConnected.value = true
}

const disconnectZoom = () => {
  zoomConnected.value = false
}

const connectGoogle = () => {
  googleConnected.value = true
}

const disconnectGoogle = () => {
  googleConnected.value = false
}
</script>
