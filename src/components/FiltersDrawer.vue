<script setup>
    import Filter from '@/components/Filter.vue'
    import { defaults } from '@/assets/filters.defaults'
    import {ref, watch} from 'vue'

    const filters = defineModel('filters')
    const draftFilters = defineModel('draftFilters')
    const hasActiveFilters = defineProps({
        hasActiveFilters: { type: Boolean, required: true },
    })

    const isFiltersOpen = ref(false)
    const openFilters = () => (isFiltersOpen.value = true)
    const closeFilters = () => (isFiltersOpen.value = false)

    const resetAllFilters = () => {
        Object.assign(filters.value, structuredClone(defaults()))
        Object.assign(draftFilters.value, structuredClone(defaults()))
    }


    const applyFilters = () => {
        filters.value.area = [...draftFilters.value.area]
        filters.value.rooms = [...draftFilters.value.rooms]
        filters.value.address = draftFilters.value.address
    }

    const onApply = () => {
        applyFilters()
        closeFilters()
    }

    watch(isFiltersOpen, (open) => {
        document.body.style.overflow = open ? 'hidden' : ''
    })
</script>

<template>
    <button
        @click="openFilters"
        class="inline-flex items-center gap-2 px-4 py-2 rounded-lg bg-white border border-gray-200 shadow-sm hover:bg-gray-50"
    >
        <span>Filters</span>
        <span v-if="hasActiveFilters" class="text-xs px-2 py-0.5 rounded-full bg-blue-100 text-blue-800">
            Active
        </span>
    </button>
    <Teleport to="body">
    <div
        v-if="isFiltersOpen"
        class="fixed inset-0 z-40 bg-black/40"
        @click="closeFilters"
    ></div>

    <div
        class="fixed inset-y-0 right-0 z-50 w-[92vw] sm:w-[420px] lg:w-[460px] bg-white shadow-xl border-l border-gray-200 transition-transform flex flex-col"
        :class="isFiltersOpen ? 'translate-x-0' : 'translate-x-full'"
        role="dialog"
        aria-modal="true"
    >
        <div class="p-4 border-b border-gray-200 flex items-center justify-between">
            <h2 class="text-lg font-bold text-gray-800">Filters</h2>
            <button
                @click="closeFilters"
                class="w-9 h-9 rounded-lg hover:bg-gray-100 flex items-center justify-center"
                aria-label="Close filters"
            >
                ✕
            </button>
        </div>
        <form @submit.prevent="onApply" class="flex flex-col h-full min-h-0">
            <div class="p-4 overflow-y-auto flex-1 min-h-0">
                <Filter
                    v-model="draftFilters.area"
                    type="text-range"
                    input-id="d-area-id"
                    label="Area:"
                    :min="0"
                    :max="100"
                    :placeholder="['From','To']"
                />
                <Filter
                    v-model="draftFilters.rooms"
                    type="slider"
                    input-id="d-rooms-id"
                    label="Number of rooms:"
                    :min="1"
                    :max="5"
                />
                <Filter
                    v-model="draftFilters.address"
                    type="text"
                    input-id="d-address-id"
                    label="Address:"
                    placeholder="City, District, Street №"
                />
            </div>
            <div class="p-4 border-t border-gray-200 flex gap-3">
                <button
                    type="button"
                    @click="resetAllFilters"
                    class="flex-1 px-4 py-2 rounded-lg border border-gray-200 bg-white hover:bg-gray-50"
                >
                    Reset
                </button>
                <button
                    type="submit"
                    class="flex-1 px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-700"
                >
                    Apply
                </button>
            </div>
        </form>
    </div>
    </Teleport>
</template>