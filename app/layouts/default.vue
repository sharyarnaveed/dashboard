<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const open = ref(false)

const links = [[{
  label: 'Dashboard',
  icon: 'i-lucide-grid-3x3',
  to: '/dashboard',
  onSelect: () => {
    open.value = false
  }
}, {
  label: 'My Classes',
  icon: 'i-lucide-book-open',
  to: '/classes',
  onSelect: () => {
    open.value = false
  }
}, {
  label: 'Upload Timetable',
  icon: 'i-lucide-upload',
  to: '/upload-timetable',
  onSelect: () => {
    open.value = false
  }
}, {
  label: 'Calendar',
  icon: 'i-lucide-calendar',
  to: '/calendar',
  onSelect: () => {
    open.value = false
  }
}, {
  label: 'Integrations',
  icon: 'i-lucide-plug',
  to: '/integrations',
  onSelect: () => {
    open.value = false
  }
}, {
  label: 'Settings',
  to: '/settings',
  icon: 'i-lucide-settings',
  onSelect: () => {
    open.value = false
  }
}]] satisfies NavigationMenuItem[][]
</script>

<template>
  <UDashboardGroup unit="rem">
    <UDashboardSidebar
      id="default"
      v-model:open="open"
      collapsible
      resizable
      class="bg-surface border-r border-border"
      :ui="{ footer: 'lg:border-t lg:border-border' }"
    >
      <template #header="{ collapsed }">
        <div class="flex items-center gap-2 px-2 py-4">
          <div class="flex h-8 w-8 items-center justify-center rounded-lg bg-primary-600">
            <span class="text-sm font-bold text-white">T</span>
          </div>
          <div v-if="!collapsed" class="flex flex-col">
            <span class="text-sm font-semibold text-text">ClassSync</span>
            <span class="text-xs text-text-secondary">Teacher Platform</span>
          </div>
        </div>
      </template>

      <template #default="{ collapsed }">
        <UNavigationMenu
          :collapsed="collapsed"
          :items="links[0]"
          orientation="vertical"
          tooltip
          popover
          class="text-text"
        />
      </template>

      <template #footer="{ collapsed }">
        <div v-if="!collapsed" class="border-t border-border p-4">
          <div class="text-xs text-text-secondary">Logged in as</div>
          <div class="mt-2 text-sm font-medium text-text">John Doe</div>
          <div class="text-xs text-text-secondary">john@school.edu</div>
        </div>
      </template>
    </UDashboardSidebar>

    <slot />
  </UDashboardGroup>
</template>
