<template>
    <div class="flex flex-col border border-gray-300 rounded-sm h-full overflow-y-auto" :class="contentType === 'input' ? 'bg-green-100' : 'bg-yellow-100'">
        <div v-for="(item, idx) in items" :key="idx" class="flex flex-row border-b border-gray-300 rounded-sm">
            <div class="w-14 flex justify-center items-center border-r border-gray-300 py-2">
                <span>{{ idx + 1 }}</span>
            </div>
            <div class="flex flex-1 justify-center items-center py-2">
                <span>{{ item }}</span>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    items: {
        type: Array,
        required: true,
    },
    contentType: {
        type: String,
        required: true,
        default: () => 'input',
    },
});
</script>