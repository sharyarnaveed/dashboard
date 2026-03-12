<template>
  <UDashboardPanel id="classes">
    <template #header>
      <UDashboardNavbar title="My Classes" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>

        <template #right>
          <UInput
            v-model="searchQuery"
            icon="i-lucide-search"
            placeholder="Search classes..."
            color="white"
            variant="none"
            :ui="{ base: 'relative block w-full disabled:cursor-not-allowed disabled:opacity-75 focus:outline-none border-0 form-input rounded-full shadow-sm bg-surface-light text-text placeholder-text-secondary focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-primary-500 focus-visible:ring-offset-background' }"
          />
        </template>
      </UDashboardNavbar>

      <UDashboardToolbar>
        <template #left>
          <UButton
            variant="outline"
            color="gray"
            :trailing-icon="filterOpen ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'"
            @click="filterOpen = !filterOpen"
          >
            Filter
          </UButton>
        </template>
      </UDashboardToolbar>

      <div v-if="filterOpen" class="border-t border-border bg-surface-light p-4">
        <div class="grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
          <div>
            <label class="block text-xs font-semibold text-text-secondary mb-2">Platform</label>
            <select v-model="selectedPlatform" class="w-full rounded border border-border bg-surface px-3 py-2 text-sm text-text">
              <option value="">All Platforms</option>
              <option value="zoom">Zoom</option>
              <option value="google">Google Meet</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-text-secondary mb-2">Day</label>
            <select v-model="selectedDay" class="w-full rounded border border-border bg-surface px-3 py-2 text-sm text-text">
              <option value="">All Days</option>
              <option value="monday">Monday</option>
              <option value="tuesday">Tuesday</option>
              <option value="wednesday">Wednesday</option>
              <option value="thursday">Thursday</option>
              <option value="friday">Friday</option>
            </select>
          </div>
        </div>
      </div>
    </template>

    <template #body>
      <div class="rounded-lg border border-border bg-surface overflow-hidden">
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead class="border-b border-border bg-surface-light">
              <tr>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Course</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Day</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Time</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Platform</th>
                <th class="px-6 py-3 text-left text-xs font-semibold text-text-secondary">Meeting Link</th>
                <th class="px-6 py-3 text-right text-xs font-semibold text-text-secondary">Actions</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-border">
              <tr v-for="cls in filteredClasses" :key="cls.id" class="hover:bg-surface-light transition">
                <td class="px-6 py-4">
                  <div>
                    <p class="font-medium text-text">{{ cls.course }}</p>
                    <p class="text-xs text-text-secondary">Section {{ cls.section }}</p>
                  </div>
                </td>
                <td class="px-6 py-4 text-text">{{ cls.day }}</td>
                <td class="px-6 py-4 text-text">{{ cls.time }}</td>
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
                  <UDropdownMenu :items="[[{ label: 'Edit', icon: 'i-lucide-edit' }, { label: 'Delete', icon: 'i-lucide-trash-2', color: 'red' }]]">
                    <UButton variant="ghost" color="gray" icon="i-lucide-ellipsis-h" />
                  </UDropdownMenu>
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
const searchQuery = ref('')
const filterOpen = ref(false)
const selectedPlatform = ref('')
const selectedDay = ref('')

const allClasses = ref([
  {
    id: 1,
    course: 'Mathematics 101',
    section: 'A',
    day: 'Monday',
    time: '09:00 AM - 10:30 AM',
    platform: 'zoom',
    meetingLink: 'https://zoom.us/j/123456789?pwd=abc123def456'
  },
  {
    id: 2,
    course: 'Physics Advanced',
    section: 'B',
    day: 'Tuesday',
    time: '11:00 AM - 12:30 PM',
    platform: 'google',
    meetingLink: 'https://meet.google.com/abc-defg-hij'
  },
  {
    id: 3,
    course: 'Chemistry Lab',
    section: 'C',
    day: 'Wednesday',
    time: '02:00 PM - 03:30 PM',
    platform: 'zoom',
    meetingLink: 'https://zoom.us/j/987654321?pwd=xyz789uvw012'
  },
  {
    id: 4,
    course: 'Biology Lecture',
    section: 'A',
    day: 'Thursday',
    time: '10:00 AM - 11:30 AM',
    platform: 'google',
    meetingLink: 'https://meet.google.com/xyz-uvwx-abc'
  },
  {
    id: 5,
    course: 'History 201',
    section: 'B',
    day: 'Friday',
    time: '01:00 PM - 02:30 PM',
    platform: 'zoom',
    meetingLink: 'https://zoom.us/j/555555555?pwd=test123456'
  }
])

const filteredClasses = computed(() => {
  return allClasses.value.filter(cls => {
    const matchesSearch = cls.course.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
                         cls.section.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesPlatform = !selectedPlatform.value || cls.platform === selectedPlatform.value
    const matchesDay = !selectedDay.value || cls.day.toLowerCase() === selectedDay.value.toLowerCase()

    return matchesSearch && matchesPlatform && matchesDay
  })
})

const copyToClipboard = (text: string) => {
  navigator.clipboard.writeText(text)
}
</script>
