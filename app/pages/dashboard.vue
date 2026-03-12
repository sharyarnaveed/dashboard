<template>
  <UDashboardPanel id="dashboard">
    <template #header>
      <UDashboardNavbar title="Dashboard" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>

        <template #right>
          <UButton
            variant="ghost"
            color="gray"
            icon="i-lucide-refresh-cw"
            @click="refreshData"
          />
        </template>
      </UDashboardNavbar>
    </template>

    <template #body>
      <!-- Stats Cards -->
      <div class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3 mb-8">
        <!-- Total Classes -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm text-text-secondary">Total Classes</p>
              <p class="mt-2 text-4xl font-bold">12</p>
            </div>
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-book-open" class="h-6 w-6 text-primary-400" />
            </div>
          </div>
          <p class="mt-4 text-xs text-text-secondary">For this semester</p>
        </div>

        <!-- Classes Today -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm text-text-secondary">Classes Today</p>
              <p class="mt-2 text-4xl font-bold">3</p>
            </div>
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-success bg-opacity-20">
              <UIcon name="i-lucide-check-circle" class="h-6 w-6 text-success" />
            </div>
          </div>
          <p class="mt-4 text-xs text-text-secondary">Starting at 09:00 AM</p>
        </div>

        <!-- Upcoming Classes -->
        <div class="rounded-lg border border-border bg-surface p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm text-text-secondary">Upcoming Classes</p>
              <p class="mt-2 text-4xl font-bold">27</p>
            </div>
            <div class="flex h-12 w-12 items-center justify-center rounded-lg bg-primary-600 bg-opacity-20">
              <UIcon name="i-lucide-calendar" class="h-6 w-6 text-primary-400" />
            </div>
          </div>
          <p class="mt-4 text-xs text-text-secondary">In the next 30 days</p>
        </div>
      </div>

      <!-- Upcoming Classes Table -->
      <div class="rounded-lg border border-border bg-surface overflow-hidden">
        <div class="border-b border-border p-6">
          <h2 class="text-lg font-semibold">Today's Classes</h2>
        </div>

        <div class="overflow-x-auto">
          <table class="w-full">
            <thead class="border-b border-border bg-surface-light">
              <tr>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Course</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Time</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Platform</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Meeting Link</th>
                <th class="px-6 py-3 text-right text-xs font-semibold text-text-secondary">Actions</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-border">
              <tr v-for="cls in todayClasses" :key="cls.id" class="hover:bg-surface-light transition">
                <td class="px-6 py-4">
                  <div>
                    <p class="font-medium text-text">{{ cls.course }}</p>
                    <p class="text-xs text-text-secondary">Section {{ cls.section }}</p>
                  </div>
                </td>
                <td class="px-6 py-4">
                  <p class="text-text">{{ cls.time }}</p>
                </td>
                <td class="px-6 py-4">
                  <div class="flex items-center gap-2">
                    <UIcon
                      :name="cls.platform === 'zoom' ? 'i-lucide-video' : 'i-lucide-phone'"
                      class="h-4 w-4 text-primary-400"
                    />
                    <span class="text-sm text-text">{{ cls.platform === 'zoom' ? 'Zoom' : 'Google Meet' }}</span>
                  </div>
                </td>
                <td class="px-6 py-4">
                  <div class="flex items-center gap-2">
                    <code class="text-xs bg-surface-light px-2 py-1 rounded text-text-secondary">
                      {{ cls.meetingLink.slice(0, 20) }}...
                    </code>
                    <button
                      @click="copyToClipboard(cls.meetingLink)"
                      class="text-primary-400 hover:text-primary-300"
                      title="Copy link"
                    >
                      <UIcon name="i-lucide-copy" class="h-4 w-4" />
                    </button>
                  </div>
                </td>
                <td class="px-6 py-4 text-right">
                  <button class="text-primary-400 hover:text-primary-300 text-sm font-medium">
                    Start Class
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </template>
  </UDashboardPanel>
</template>

<script setup lang="ts">
const todayClasses = ref([
  {
    id: 1,
    course: 'Mathematics 101',
    section: 'A',
    time: '09:00 AM - 10:30 AM',
    platform: 'zoom',
    meetingLink: 'https://zoom.us/j/123456789?pwd=abc123def456'
  },
  {
    id: 2,
    course: 'Physics Advanced',
    section: 'B',
    time: '11:00 AM - 12:30 PM',
    platform: 'google',
    meetingLink: 'https://meet.google.com/abc-defg-hij'
  },
  {
    id: 3,
    course: 'Chemistry Lab',
    section: 'C',
    time: '02:00 PM - 03:30 PM',
    platform: 'zoom',
    meetingLink: 'https://zoom.us/j/987654321?pwd=xyz789uvw012'
  }
])

const refreshData = () => {
  // Refresh logic
}

const copyToClipboard = (text: string) => {
  navigator.clipboard.writeText(text)
}
</script>
