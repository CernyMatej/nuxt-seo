<script setup lang="ts">
defineOptions({
  inheritAttrs: false,
})

const props = defineProps<{
  class?: any
}>()

const config = {
  wrapper: 'relative [&>div:last-child]:!my-0 [&>div:last-child]:!static my-5',
  header: 'flex items-center gap-1 border border-gray-200 dark:border-gray-700 border-b-0 rounded-t-md overflow-hidden p-2',
  tab: {
    base: 'px-2 py-1.5 focus:outline-none text-gray-700 dark:text-gray-200 text-sm rounded-md flex items-center gap-1.5',
    active: 'bg-gray-100 dark:bg-gray-800',
    inactive: 'hover:bg-gray-50 dark:hover:bg-gray-800/50',
    icon: {
      base: '',
    },
  },
}

const slots = useSlots()
// const { ui, attrs } = useUI('content.codeGroup', undefined, config, toRef(props, 'class'), true)

const selectedIndex = ref(0)

// Computed

const tabs = computed(() => slots.default?.().map((slot, index) => {
  return {
    label: slot.props?.filename || slot.props?.label || `${index}`,
    icon: slot.props?.icon,
    component: slot,
  }
}) || [])

const selectedTab = computed(() => tabs.value.find((_, index) => index === selectedIndex.value))
</script>

<template>
  <div>
    <div>
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        tabindex="-1"
        @click="selectedIndex = index"
      >
        <span>{{ tab.label }}</span>
      </button>
    </div>

    <component :is="selectedTab?.component" v-if="selectedTab" hide-header />
  </div>
</template>
