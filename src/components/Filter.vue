<template>
  <div class="mb-4">
    <!-- Фильтр двойной input(range) -->
    <div v-if="type === 'text-range'">
      <label :for="inputId" class="block text-sm font-medium mb-2 text-gray-700">{{ label }}</label>
      <div class="flex flex-col sm:flex-row sm:items-center gap-2">
        <input 
          type="text" 
          :id="inputId" 
          v-model="model[0]"
          :placeholder="placeholder[0]"
          class="w-24 sm:w-28 px-2 py-1.5 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        />   
      <span class="text-gray-500 text-sm">—</span>
        <input
          type="text"
          :id="`${inputId}-to`"
          v-model="model[1]"
          :placeholder="placeholder[1]"
          class="w-24 sm:w-28 px-2 py-1.5 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
    </div>

    <!-- Фильтр одиночный input -->
    <div v-else-if="type === 'text'">
      <div>
        <label :for="inputId" class="block text-sm font-medium mb-1 text-gray-700">{{ label }}</label>
        <input 
          type="text" 
          :id="inputId" 
          v-model="model"
          :placeholder="placeholder"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        /> 
      </div>
    </div>

    <!-- Фильтр двойной slider -->
    <div v-else-if="type === 'slider'">
      <div>
        <label :for="inputId" class="block text-sm font-medium mb-2 text-gray-700">{{ label }}</label>
        
        <div class="flex items-center justify-between mb-3">
          <span class="font-medium text-sm text-gray-700">{{ Math.min(value1, value2) }}</span>
          <span class="font-medium text-sm text-gray-700">{{ Math.max(value1, value2) }}</span>
        </div>
        
        <div class="relative h-6 mb-2">
          <div class="absolute top-1/2 -translate-y-1/2 w-full h-1.5 bg-gray-200 rounded-full"></div>
          
          <div 
            class="absolute top-1/2 -translate-y-1/2 h-1.5 bg-blue-500 rounded-full"
            :style="{
              left: `${((Math.min(value1, value2) - min) / (max - min)) * 100}%`,
              right: `${100 - ((Math.max(value1, value2) - min) / (max - min)) * 100}%`
            }"
          ></div>
          
          <input 
            type="range" 
            :id="`${inputId}-1`" 
            :min="min" 
            :max="max" 
            v-model.number="value1"
            @change="handleSlider1Change"
            class="absolute w-full top-1/2 -translate-y-1/2 bg-transparent appearance-none pointer-events-none 
                   [&::-webkit-slider-thumb]:pointer-events-auto 
                   [&::-webkit-slider-thumb]:appearance-none 
                   [&::-webkit-slider-thumb]:w-5 
                   [&::-webkit-slider-thumb]:h-5 
                   [&::-webkit-slider-thumb]:rounded-full
                   [&::-webkit-slider-thumb]:bg-blue-500 
                   [&::-webkit-slider-thumb]:cursor-pointer 
                   [&::-webkit-slider-thumb]:border-2 
                   [&::-webkit-slider-thumb]:border-white 
                   [&::-webkit-slider-thumb]:shadow-md
                   [&::-webkit-slider-thumb]:transition-all
                   [&::-webkit-slider-thumb]:hover:scale-110
                   [&::-webkit-slider-thumb]:hover:bg-blue-600
                   [&::-webkit-slider-thumb]:active:scale-95
                   [&::-moz-range-thumb]:pointer-events-auto 
                   [&::-moz-range-thumb]:appearance-none 
                   [&::-moz-range-thumb]:w-5 
                   [&::-moz-range-thumb]:h-5 
                   [&::-moz-range-thumb]:rounded-full
                   [&::-moz-range-thumb]:bg-blue-500 
                   [&::-moz-range-thumb]:cursor-pointer 
                   [&::-moz-range-thumb]:border-2 
                   [&::-moz-range-thumb]:border-white 
                   [&::-moz-range-thumb]:shadow-md
                   [&::-moz-range-thumb]:transition-all
                   [&::-moz-range-thumb]:hover:scale-110
                   [&::-moz-range-thumb]:hover:bg-blue-600"
          />
          
          <input 
            type="range" 
            :id="`${inputId}-2`"
            :min="min" 
            :max="max" 
            v-model.number="value2"
            @change="handleSlider2Change"
            class="absolute w-full top-1/2 -translate-y-1/2 bg-transparent appearance-none pointer-events-none 
                   [&::-webkit-slider-thumb]:pointer-events-auto 
                   [&::-webkit-slider-thumb]:appearance-none 
                   [&::-webkit-slider-thumb]:w-5 
                   [&::-webkit-slider-thumb]:h-5 
                   [&::-webkit-slider-thumb]:rounded-full
                   [&::-webkit-slider-thumb]:bg-blue-500 
                   [&::-webkit-slider-thumb]:cursor-pointer 
                   [&::-webkit-slider-thumb]:border-2 
                   [&::-webkit-slider-thumb]:border-white 
                   [&::-webkit-slider-thumb]:shadow-md
                   [&::-webkit-slider-thumb]:transition-all
                   [&::-webkit-slider-thumb]:hover:scale-110
                   [&::-webkit-slider-thumb]:hover:bg-blue-600
                   [&::-webkit-slider-thumb]:active:scale-95
                   [&::-moz-range-thumb]:pointer-events-auto 
                   [&::-moz-range-thumb]:appearance-none 
                   [&::-moz-range-thumb]:w-5 
                   [&::-moz-range-thumb]:h-5 
                   [&::-moz-range-thumb]:rounded-full
                   [&::-moz-range-thumb]:bg-blue-500 
                   [&::-moz-range-thumb]:cursor-pointer 
                   [&::-moz-range-thumb]:border-2 
                   [&::-moz-range-thumb]:border-white 
                   [&::-moz-range-thumb]:shadow-md
                   [&::-moz-range-thumb]:transition-all
                   [&::-moz-range-thumb]:hover:scale-110
                   [&::-moz-range-thumb]:hover:bg-blue-600"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, watch, computed } from 'vue'

  const {inputId, type, label, min, max, placeholder} = defineProps({
      type: {
        type: String,
        required: true
      },
      inputId: {
        type: String,
        required: true
      },
      label: {
        type: String,
        required: true
      },
      min: {
        type: Number,
        required: false
      },
      max: {
        type: Number,
        required: false
      },
      placeholder: {
        type: [String, Array],
        required: false
      }
  })
  
  const model = defineModel()

  const isSlider = computed(() => type === 'slider')
  
  const value1 = ref(0)
  const value2 = ref(0)

  if (isSlider.value) {
    value1.value = model.value?.[0] ?? min
    value2.value = model.value?.[1] ?? max

    watch(model, (newValue) => {
      if (!Array.isArray(newValue)) return
      value1.value = newValue[0]
      value2.value = newValue[1]
    }, { deep: false })
  }

  const handleSlider1Change = () => {
    model.value = [Math.min(value1.value, value2.value), Math.max(value1.value, value2.value)]
  }

  const handleSlider2Change = () => {
    model.value = [Math.min(value1.value, value2.value), Math.max(value1.value, value2.value)]
  }
</script>