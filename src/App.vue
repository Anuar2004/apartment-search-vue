<script setup>
  import ApartmentsList from '@/components/ApartmentsList.vue'
  import FilterLabel from '@/components/FilterLabel.vue'
  import FiltersDrawer from '@/components/FiltersDrawer.vue'
  import data from '@/assets/apartments.json'
  import {defaults} from '@/assets/filters.defaults.js'
  import {ref, reactive, computed} from 'vue'

  const apartments = ref(Object.values(data))

  const draftFilters = reactive(defaults())
  const appliedFilters = reactive(defaults())

  const isDefault = (value, def) => {
    if (Array.isArray(value) && Array.isArray(def)) {
      return value.length === def.length && value.every((v,i)=>v===def[i])
    }
    return value === def
  }

  const hasActiveFilters = computed(() => {
    const def = defaults()
    return Object.keys(def).some((k) => !isDefault(appliedFilters[k], def[k]))
  })
</script>

<template>
  <main class="min-h-screen p-3 sm:p-5 bg-gradient-to-br from-gray-50 to-gray-100">
    <section class="flex flex-col"> 
      <div class="bg-white border border-gray-200 rounded-xl pt-6 pb-4 px-4 mb-4 shadow-sm">

        <div v-if="hasActiveFilters" class="flex flex-wrap gap-2 max-h-28 overflow-y-auto">
          <FilterLabel
            v-model:filters="appliedFilters.area" 
            v-model:draft="draftFilters.area"
            :label="`Area: ${appliedFilters.area[0]} - ${appliedFilters.area[1]}`"
            :defaultValue="defaults().area"
          />
          <FilterLabel
            v-model:filters="appliedFilters.rooms"
            v-model:draft="draftFilters.rooms"
            :label="`Rooms: ${appliedFilters.rooms[0]} - ${appliedFilters.rooms[1]}`"
            :defaultValue="defaults().rooms"
          />
          <FilterLabel
            v-model:filters="appliedFilters.address"
            v-model:draft="draftFilters.address"
            :label="`Address: ${appliedFilters.address}`"
            :defaultValue="defaults().address"
          />
        </div>

        <div v-else class="text-center text-gray-400 py-2">
          <p class="text-sm">No filters applied ¯\_(ツ)_/¯</p>
        </div>
      </div>

      <div class="flex flex-col border border-gray-200 rounded-xl bg-white shadow-sm flex-1 min-h-0">
        <div class="p-5 border-b border-gray-200 flex items-center justify-between gap-3">
          <h2 class="text-xl font-bold text-gray-800">Available apartments</h2>
          <FiltersDrawer v-model:filters="appliedFilters" v-model:draftFilters="draftFilters" :has-active-filters="hasActiveFilters"/>
        </div>
        
        <div class="p-5 overflow-y-auto flex-1">
          <ApartmentsList :filters="appliedFilters" :apartments="apartments"/>
        </div>
      </div>
    </section>
  </main>
</template>