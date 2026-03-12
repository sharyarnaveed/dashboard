<template>
  <UDashboardPanel id="settings">
    <template #header>
      <UDashboardNavbar title="Settings" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>
      </UDashboardNavbar>

      <UDashboardToolbar>
        <template #left>
          <div class="flex gap-2">
            <button
              v-for="tab in tabs"
              :key="tab"
              @click="activeTab = tab"
              class="px-4 py-2 text-sm font-medium transition rounded-lg"
              :class="activeTab === tab ? 'bg-primary-600 text-white' : 'text-text-secondary hover:text-text'"
            >
              {{ tab }}
            </button>
          </div>
        </template>
      </UDashboardToolbar>
    </template>

    <template #body>
      <!-- Profile Tab -->
      <div v-if="activeTab === 'Profile'" class="max-w-2xl space-y-8">
        <div>
          <h2 class="text-lg font-semibold text-text mb-6">Profile Information</h2>

          <form @submit.prevent="saveProfile" class="space-y-4">
            <!-- Full Name -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Full Name</label>
              <input
                v-model="profile.fullName"
                type="text"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>

            <!-- Email -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Email Address</label>
              <input
                v-model="profile.email"
                type="email"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>

            <!-- Institution -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">School/Institution</label>
              <input
                v-model="profile.institution"
                type="text"
                placeholder="e.g. ABC School"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>

            <!-- Phone -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Phone Number</label>
              <input
                v-model="profile.phone"
                type="tel"
                placeholder="+1 (555) 000-0000"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>

            <!-- Bio -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Bio</label>
              <textarea
                v-model="profile.bio"
                rows="4"
                placeholder="Tell us about yourself..."
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              ></textarea>
            </div>

            <button
              type="submit"
              class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition"
            >
              Save Changes
            </button>
          </form>
        </div>
      </div>

      <!-- Preferences Tab -->
      <div v-if="activeTab === 'Preferences'" class="max-w-2xl space-y-8">
        <div>
          <h2 class="text-lg font-semibold text-text mb-6">Preferences</h2>

          <div class="space-y-6">
            <!-- Default Platform -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Default Meeting Platform</label>
              <select v-model="preferences.defaultPlatform" class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text">
                <option value="zoom">Zoom</option>
                <option value="google">Google Meet</option>
              </select>
            </div>

            <!-- Timezone -->
            <div>
              <label class="block text-sm font-medium text-text mb-2">Timezone</label>
              <select v-model="preferences.timezone" class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text">
                <option value="utc">UTC (Coordinated Universal Time)</option>
                <option value="est">EST (Eastern Standard Time)</option>
                <option value="cst">CST (Central Standard Time)</option>
                <option value="mst">MST (Mountain Standard Time)</option>
                <option value="pst">PST (Pacific Standard Time)</option>
              </select>
            </div>

            <!-- Notifications -->
            <div>
              <h3 class="text-sm font-medium text-text mb-3">Notifications</h3>
              <div class="space-y-3">
                <label class="flex items-center gap-3">
                  <input v-model="preferences.classReminders" type="checkbox" class="rounded border border-border" />
                  <span class="text-sm text-text">Class reminder notifications</span>
                </label>
                <label class="flex items-center gap-3">
                  <input v-model="preferences.newClassAlerts" type="checkbox" class="rounded border border-border" />
                  <span class="text-sm text-text">New class alerts</span>
                </label>
                <label class="flex items-center gap-3">
                  <input v-model="preferences.weeklyDigest" type="checkbox" class="rounded border border-border" />
                  <span class="text-sm text-text">Weekly digest email</span>
                </label>
              </div>
            </div>

            <button
              @click="savePreferences"
              class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition"
            >
              Save Preferences
            </button>
          </div>
        </div>
      </div>

      <!-- Danger Zone Tab -->
      <div v-if="activeTab === 'Danger Zone'" class="max-w-2xl space-y-8">
        <div class="rounded-lg border border-error border-opacity-20 bg-error bg-opacity-10 p-6">
          <h2 class="text-lg font-semibold text-error mb-4">Danger Zone</h2>
          <p class="text-sm text-text-secondary mb-6">These actions are irreversible. Please proceed with caution.</p>

          <!-- Change Password -->
          <div class="mb-6 space-y-4 pb-6 border-b border-error border-opacity-20">
            <h3 class="font-medium text-text">Change Password</h3>
            <div>
              <label class="block text-sm text-text-secondary mb-2">Current Password</label>
              <input
                v-model="dangerZone.currentPassword"
                type="password"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>
            <div>
              <label class="block text-sm text-text-secondary mb-2">New Password</label>
              <input
                v-model="dangerZone.newPassword"
                type="password"
                class="w-full rounded-lg border border-border bg-surface-light px-4 py-2 text-text placeholder-text-secondary focus:border-primary-500 focus:ring-2 focus:ring-primary-500 focus:ring-opacity-20 outline-none transition"
              />
            </div>
            <button class="rounded-lg bg-primary-600 px-6 py-2 font-medium text-white hover:bg-primary-700 transition">
              Update Password
            </button>
          </div>

          <!-- Delete Account -->
          <div>
            <h3 class="font-medium text-error mb-4">Delete Account</h3>
            <p class="text-sm text-text-secondary mb-4">
              Deleting your account will permanently remove all your data and cannot be undone.
            </p>
            <button class="rounded-lg bg-error px-6 py-2 font-medium text-white hover:bg-opacity-90 transition">
              Delete Account
            </button>
          </div>
        </div>
      </div>
    </template>
  </UDashboardPanel>
</template>

<script setup lang="ts">
const activeTab = ref('Profile')
const tabs = ['Profile', 'Preferences', 'Danger Zone']

const profile = ref({
  fullName: 'John Doe',
  email: 'john@school.edu',
  institution: 'ABC School',
  phone: '+1 (555) 000-0000',
  bio: 'Experienced mathematics teacher passionate about online education.'
})

const preferences = ref({
  defaultPlatform: 'zoom',
  timezone: 'est',
  classReminders: true,
  newClassAlerts: true,
  weeklyDigest: true
})

const dangerZone = ref({
  currentPassword: '',
  newPassword: ''
})

const saveProfile = () => {
  // Save logic
}

const savePreferences = () => {
  // Save logic
}
</script>
