<script setup>
    import { computed } from "vue";

    const {filters, apartments} = defineProps({
        filters: {
            type: Object,
            required: true
        },
        apartments: {
            type: Array,
            required: true
        }
    })

    const filteredApartments = computed(() => {
        return apartments.filter((apartment) => {
            for (const [key, filterValue] of Object.entries(filters)) {
                const dbValue = apartment[key]
                if (dbValue == null) continue

                if (Array.isArray(filterValue)) {
                    const [min, max] = filterValue

                    if (min === '' && max === '') continue

                    if (min !== '' && Number(dbValue) < Number(min)) return false
                    if (max !== '' && Number(dbValue) > Number(max)) return false
                    
                    continue
                }

                if (typeof filterValue === 'string') {
                    const cleanString = filterValue.trim()
                    if (cleanString === '') continue
                    if (!String(dbValue).toLowerCase().includes(cleanString.toLowerCase())) return false

                    continue
                }
            }
            return true
        })
    })
</script>

<template>
    <div class="flex flex-col gap-4">
        <div v-if="filteredApartments.length === 0" class="text-center text-gray-500 mt-16 py-8">
            <p class="text-xl">No apartments found ¯\_(ツ)_/¯</p>
        </div>
        <div v-else class="flex flex-col gap-4">
            <div
                class="flex flex-col sm:flex-row border border-gray-200 rounded-lg overflow-hidden bg-white hover:shadow-md transition-shadow"
                v-for="apartment in filteredApartments"
                :key="apartment.id"
            >
                <div class="flex-1 p-5">
                    <h3 class="text-lg font-semibold text-gray-800 mb-3">{{ apartment.address }}</h3>
                    <div class="space-y-2">
                        <div v-for="(value, key) in apartment" :key="key">
                            <p v-if="key !== 'image' && key !== 'address'" class="text-sm text-gray-600">
                                <span class="font-medium text-gray-700 capitalize">{{ key }}:</span> {{ value }}
                            </p>
                        </div>
                    </div>
                </div>

                <div class="w-full h-48 sm:w-64 flex-shrink-0">
                    <img 
                        v-if="apartment.image" 
                        :src="apartment.image" 
                        :alt="apartment.address" 
                        class="w-full h-full object-cover"
                    />
                    <div v-else class="w-full h-full bg-gray-100 flex items-center justify-center">
                        <span class="text-gray-400">No image</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>