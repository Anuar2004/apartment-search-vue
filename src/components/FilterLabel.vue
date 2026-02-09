<template>
    <div v-if="!isDefault" class="inline-flex items-center gap-2 px-3 py-1.5 bg-blue-100 text-blue-800 rounded-full text-sm">
        <label class="font-medium">{{ label }}</label>  
        <button
            @click="resetFilter"
            title="reset"
            class="hover:bg-blue-200 rounded-full w-5 h-5 flex items-center justify-center transition-colors"
        >
            ✕ 
        </button>
    </div>
</template>

<script setup>
    import {computed} from 'vue'

    const {label, defaultValue} = defineProps({
        label: {
            type: String,
            required: true
        },
        defaultValue: {
            required: true
        }
    })

    const filters = defineModel('filters')
    const draft = defineModel('draft')

    const isDefault = computed(() => {
        const a = filters.value
        const b = defaultValue

        if (Array.isArray(a) && Array.isArray(b)) {
            return a.length === b.length && a.every((element, index) => element === b[index])
        }
        return a === b
    })

    const resetFilter = () => {
        filters.value = (defaultValue && typeof defaultValue === 'object') ? structuredClone(defaultValue) : defaultValue
        draft.value = (defaultValue && typeof defaultValue === 'object') ? structuredClone(defaultValue) : defaultValue
    }
</script>