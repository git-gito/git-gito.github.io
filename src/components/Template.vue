<template>
    <div :class="isSelected
        ? 'mb-4 p-6 border border-blue-500 bg-blue-50 rounded-lg'
        : 'template-item bg-white transition-shadow border border-gray-200 rounded-lg mb-4 p-6 cursor-pointer'"
        @click="$emit('select', template)">
        <h3 class="text-lg font-semibold mb-2">{{ template.title }}</h3>
        <p class="text-gray-600 mb-3">{{ template.description }}</p>
        <p class="text-xs text-gray-500 mb-2">Owner: {{ template.owner }}</p>
        <code class="bg-gray-100 px-2 py-1 rounded text-sm">gito template {{ template.name }}</code>
    </div>
</template>

<script setup>
import { computed } from 'vue'
const props = defineProps({
    template: {
        type: Object,
        required: true
    },
    selectedTemplate: {
        type: Object,
        default: null
    }
})
// test build
const isSelected = computed(() => {
    return props.selectedTemplate?.name === props.template.name
})
</script>
