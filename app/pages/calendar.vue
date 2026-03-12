<template>
  <UDashboardPanel id="calendar">
    <template #header>
      <UDashboardNavbar title="Calendar" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>

        <template #right>
          <div class="flex gap-2">
            <button @click="previousWeek" class="rounded-lg border border-border px-3 py-2 text-text hover:bg-surface-light transition">
              <UIcon name="i-lucide-chevron-left" class="h-5 w-5" />
            </button>
            <button @click="nextWeek" class="rounded-lg border border-border px-3 py-2 text-text hover:bg-surface-light transition">
              <UIcon name="i-lucide-chevron-right" class="h-5 w-5" />
            </button>
          </div>
        </template>
      </UDashboardNavbar>
    </template>

    <template #body>
      <div class="space-y-6">
        <!-- Week Display -->
        <div class="text-center">
          <h2 class="text-xl font-semibold">Week of {{ currentWeekStart }}</h2>
        </div>

        <!-- Calendar Grid -->
        <div class="grid gap-4 grid-cols-1 lg:grid-cols-7">
          <div v-for="day in weekDays" :key="day.date" class="rounded-lg border border-border bg-surface p-4">
            <div class="mb-4">
              <p class="text-sm font-medium text-text-secondary">{{ day.dayName }}</p>
              <p class="text-2xl font-bold text-text">{{ day.date }}</p>
            </div>

            <div class="space-y-3">
              <div
                v-for="cls in day.classes"
                :key="cls.id"
                @click="selectedClass = cls"
                class="cursor-pointer rounded-lg bg-surface-light p-3 hover:bg-primary-600 hover:bg-opacity-20 transition"
              >
                <p class="text-sm font-medium text-text">{{ cls.course }}</p>
                <p class="text-xs text-text-secondary mt-1">{{ cls.time }}</p>
                <div class="flex items-center gap-1 mt-2">
                  <UIcon
                    :name="cls.platform === 'zoom' ? 'i-lucide-video' : 'i-lucide-phone'"
                    class="h-3 w-3 text-primary-400"
                  />
                  <span class="text-xs text-primary-400">{{ cls.platform === 'zoom' ? 'Zoom' : 'Meet' }}</span>
                </div>
              </div>

              <div v-if="day.classes.length === 0" class="text-center py-6">
                <p class="text-xs text-text-secondary">No classes</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Class Details Modal -->
        <Teleport to="body" v-if="selectedClass">
          <div
            @click="selectedClass = null"
            class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4"
          >
            <div
              @click.stop
              class="rounded-lg border border-border bg-surface max-w-md w-full p-6 shadow-xl"
            >
              <div class="flex items-start justify-between mb-4">
                <div>
                  <h3 class="text-xl font-bold text-text">{{ selectedClass.course }}</h3>
                  <p class="text-sm text-text-secondary mt-1">Section {{ selectedClass.section }}</p>
                </div>
                <button
                  @click="selectedClass = null"
                  class="text-text-secondary hover:text-text transition"
                >
                  <UIcon name="i-lucide-x" class="h-6 w-6" />
                </button>
              </div>

              <div class="space-y-4">
                <div>
                  <p class="text-xs text-text-secondary mb-1">Time</p>
                  <p class="text-text">{{ selectedClass.time }}</p>
                </div>

                <div>
                  <p class="text-xs text-text-secondary mb-1">Platform</p>
                  <div class="flex items-center gap-2">
                    <UIcon
                      :name="selectedClass.platform === 'zoom' ? 'i-lucide-video' : 'i-lucide-phone'"
                      class="h-4 w-4 text-primary-400"
                    />
                    <span class="text-text">{{ selectedClass.platform === 'zoom' ? 'Zoom' : 'Google Meet' }}</span>
                  </div>
                </div>

                <div>
                  <p class="text-xs text-text-secondary mb-1">Meeting Link</p>
                  <div class="flex items-center gap-2">
                    <code class="text-xs bg-surface-light px-2 py-1 rounded text-text-secondary flex-1 truncate">
                      {{ selectedClass.meetingLink }}
                    </code>
                    <button
                      @click="copyToClipboard(selectedClass.meetingLink)"
                      class="text-primary-400 hover:text-primary-300"
                    >
                      <UIcon name="i-lucide-copy" class="h-4 w-4" />
                    </button>
                  </div>
                </div>

                <div class="flex gap-3 pt-4 border-t border-border">
                  <button class="flex-1 rounded-lg bg-primary-600 px-4 py-2 font-medium text-white hover:bg-primary-700 transition">
                    Start Class
                  </button>
                  <button
                    @click="selectedClass = null"
                    class="flex-1 rounded-lg border border-border px-4 py-2 font-medium text-text hover:bg-surface-light transition"
                  >
                    Close
                  </button>
                </div>
              </div>
            </div>
          </div>
        </Teleport>
      </div>
    </template>
  </UDashboardPanel>
</template>

<script setup lang="ts">
import { addDays, format, startOfWeek } from 'date-fns'

const currentDate = ref(new Date())
const selectedClass = ref<any>(null)

const classes = ref([
  { id: 1, course: 'Mathematics 101', section: 'A', day: 0, time: '09:00 AM - 10:30 AM', platform: 'zoom', meetingLink: 'https://zoom.us/j/123456789' },
  { id: 2, course: 'Physics Advanced', section: 'B', day: 1, time: '11:00 AM - 12:30 PM', platform: 'google', meetingLink: 'https://meet.google.com/abc-def' },
  { id: 3, course: 'Chemistry Lab', section: 'C', day: 2, time: '02:00 PM - 03:30 PM', platform: 'zoom', meetingLink: 'https://zoom.us/j/987654' },
  { id: 4, course: 'Biology Lecture', section: 'A', day: 3, time: '10:00 AM - 11:30 AM', platform: 'google', meetingLink: 'https://meet.google.com/xyz' },
  { id: 5, course: 'History 201', section: 'B', day: 4, time: '01:00 PM - 02:30 PM', platform: 'zoom', meetingLink: 'https://zoom.us/j/555555' },
  { id: 6, course: 'English Literature', section: 'C', day: 0, time: '01:00 PM - 02:30 PM', platform: 'google', meetingLink: 'https://meet.google.com/eng' },
])

const currentWeekStart = computed(() => {
  const weekStart = startOfWeek(currentDate.value)
  return format(weekStart, 'MMM dd, yyyy')
})

const weekDays = computed(() => {
  const start = startOfWeek(currentDate.value)
  const dayNames = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

  return Array.from({ length: 7 }).map((_, i) => {
    const date = addDays(start, i)
    const dateNum = date.getDate()
    const dayClasses = classes.value.filter(cls => cls.day === i)

    return {
      dayName: dayNames[i],
      date: dateNum,
      fullDate: date,
      classes: dayClasses
    }
  })
})

const previousWeek = () => {
  currentDate.value = addDays(currentDate.value, -7)
}

const nextWeek = () => {
  currentDate.value = addDays(currentDate.value, 7)
}

const copyToClipboard = (text: string) => {
  navigator.clipboard.writeText(text)
}
</script>
