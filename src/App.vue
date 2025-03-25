<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const STORAGE_KEY = 'color-palettes'

const searchQuery = ref('')
const palettes = ref([
  {
    colors: ['#2D3250', '#424769', '#7077A1', '#F6B17A'],
    name: 'Modern Navy & Orange',
    likes: 2723,
    timeAgo: '3 weeks'
  },
  {
    colors: ['#1A1A2E', '#16213E', '#0F3460', '#E94560'],
    name: 'Dark Mode Accent',
    likes: 2599,
    timeAgo: '2 weeks'
  },
  {
    colors: ['#222831', '#393E46', '#00ADB5', '#EEEEEE'],
    name: 'Minimal Teal',
    likes: 2242,
    timeAgo: '3 weeks'
  },
  {
    colors: ['#2C3639', '#3F4E4F', '#A27B5C', '#DCD7C9'],
    name: 'Earth Tones',
    likes: 2132,
    timeAgo: '2 weeks'
  },
  {
    colors: ['#082032', '#2C394B', '#334756', '#FF4C29'],
    name: 'Dark & Fire',
    likes: 1887,
    timeAgo: '3 weeks'
  },
  {
    colors: ['#161853', '#292C6D', '#EC255A', '#FFC2D1'],
    name: 'Navy & Pink',
    likes: 1723,
    timeAgo: '3 weeks'
  }
])

// Load palettes from localStorage on component mount
onMounted(() => {
  const savedPalettes = localStorage.getItem(STORAGE_KEY)
  if (savedPalettes) {
    palettes.value = JSON.parse(savedPalettes)
  }
})

// Watch for changes in palettes and save to localStorage
watch(
  palettes,
  (newPalettes) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newPalettes))
  },
  { deep: true }
)

const filteredPalettes = computed(() => {
  return palettes.value.filter(palette => 
    palette.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const copiedColorNotification = ref({
  active: false,
  type: 'success',
  message: 'Color copied to clipboard!'
})

const copyColor = (color) => {
  navigator.clipboard.writeText(color)
  showNotification()
}

const showNotification = () => {
  copiedColorNotification.value.active = true
  setTimeout(() => {
    copiedColorNotification.value.active = false
  }, 2000)
}

const generateNewPalette = () => {
  const newPalette = {
    colors: Array(4).fill().map(() => 
      '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0')
    ),
    name: 'New Palette',
    likes: 0,
    timeAgo: 'just now'
  }
  palettes.value.unshift(newPalette)
}

const deletePalette = (palette) => {
  palettes.value.splice(palettes.value.indexOf(palette), 1)
}

const copyAllColors = (colors) => {
  navigator.clipboard.writeText(colors.join(', '))
  showNotification()
}

const updatePaletteName = (palette) => {
  const index = palettes.value.findIndex(p => p === palette)
  if (index !== -1) {
    palettes.value[index] = { ...palette }
  }
}

// Add social media links
const socialLinks = {
  github: 'https://github.com/TonyWTillet',
  linkedin: 'https://www.linkedin.com/in/tonytilletdevweb/'
}
</script>

<template>
  <div class="min-h-screen bg-gray-900 text-gray-100">
    <!-- Header -->
    <header class="border-b border-gray-800 bg-gray-900/50 backdrop-blur-sm fixed w-full top-0 z-50">
      <div class="container mx-auto px-4 py-4">
        <div class="flex items-center justify-between">
          <!-- Left side: Logo and Search -->
          <div class="flex items-center gap-4 flex-1">
            <h1 class="text-2xl font-bold bg-gradient-to-r from-purple-400 to-pink-500 bg-clip-text text-transparent whitespace-nowrap">
              Color Palette
            </h1>
            <div class="relative max-w-md w-full">
              <input 
                type="text" 
                v-model="searchQuery"
                placeholder="Search palettes" 
                class="w-full px-4 py-2 bg-gray-800/50 border border-gray-700 rounded-lg focus:outline-none focus:border-indigo-500 text-sm"
              >
            </div>
          </div>
          
          <!-- Right side: Social Links and Generate Button -->
          <div class="flex items-center gap-3">
            <a :href="socialLinks.github" 
               target="_blank" 
               class="p-2 text-gray-400 hover:text-white transition-colors duration-200">
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
              </svg>
            </a>
            <a :href="socialLinks.linkedin" 
               target="_blank" 
               class="p-2 text-gray-400 hover:text-white transition-colors duration-200">
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
              </svg>
            </a>
            <div class="w-px h-6 bg-gray-700 mx-2"></div>
            <button @click="generateNewPalette" 
                    class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 rounded-lg transition-colors duration-200 text-sm font-medium">
              Generate New
            </button>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 pt-24 pb-12">
      <!-- Grid of Palettes -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
        <div v-for="(palette, index) in filteredPalettes" 
             :key="index"
             class="bg-gray-800/30 rounded-xl overflow-hidden backdrop-blur-sm border border-gray-800 hover:border-gray-700 transition-all duration-300 group">
          <!-- Color Blocks -->
          <div class="grid grid-rows-4 h-48 cursor-pointer">
            <div v-for="(color, colorIndex) in palette.colors" 
                 :key="colorIndex"
                 :style="{ backgroundColor: color }"
                 @click="copyColor(color)"
                 class="relative transition-transform group-hover:scale-[1.02] duration-200">
              <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 bg-black/20 transition-opacity">
                <span class="font-mono text-sm bg-black/40 px-2 py-1 rounded">
                  {{ color.toUpperCase() }}
                </span>
              </div>
            </div>
          </div>
          <!-- Palette Info -->
          <div class="p-3 flex items-center justify-between text-sm">
            <div>
              <input class="text-gray-400"  v-model="palette.name" @input="updatePaletteName(palette)" />
            </div>
            <div class="flex items-center gap-2">
              <button type="button" class="text-purple-400 hover:text-pink-500 transition-colors" @click="copyAllColors(palette.colors)">Copied all</button>
              <div class="w-px h-6 bg-gray-700 mx-2"></div>
              <button type="button" class="text-gray-400 hover:text-pink-500 transition-colors" @click="deletePalette(palette)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Notification -->
    <div v-if="copiedColorNotification.active"
         class="fixed bottom-4 right-4 bg-green-500/90 text-white px-4 py-2 rounded-lg backdrop-blur-sm">
      {{ copiedColorNotification.message }}
    </div>
  </div>
</template>

<style scoped>
/* Add cursor pointer to all interactive elements */
button,
a[href],
input[type="text"],
.cursor-pointer {
  cursor: pointer;
}
</style>
