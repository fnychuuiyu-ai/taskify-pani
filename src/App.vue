<script setup>
import { ref, computed } from 'vue'

// State data tugas awal
const tasks = ref([
  { id: 1, title: 'Beli susu & camilan kucing 🐾', status: true, priority: 'Penting! 💖' },
  { id: 2, title: 'Selesaikan proyek Taskify Vue ✨', status: false, priority: 'Penting! 💖' },
  { id: 3, title: 'Olahraga sore 20 menit 🌸', status: false, priority: 'Santai 🌿' }
])

const inputTask = ref('')
const selectedPriority = ref('Penting! 💖')
const currentFilter = ref('all')

// Tambah Tugas
const addTask = () => {
  if (!inputTask.value.trim()) return

  const newTask = {
    id: Date.now(),
    title: inputTask.value,
    status: false,
    priority: selectedPriority.value
  }

  tasks.value.unshift(newTask)
  inputTask.value = ''
}

// Hapus Tugas
const deleteTask = (id) => {
  if (confirm('Yakin mau hapus tugas manis ini? 🥺')) {
    tasks.value = tasks.value.filter(item => item.id !== id)
  }
}

// Bersihkan Tugas Selesai
const clearCompleted = () => {
  tasks.value = tasks.value.filter(t => !t.status)
}

// Hitung Progress Persentase
const completedCount = computed(() => tasks.value.filter(t => t.status).length)
const progressPercentage = computed(() => {
  if (tasks.value.length === 0) return 0
  return Math.round((completedCount.value / tasks.value.length) * 100)
})

// Filter Tugas
const filteredTasks = computed(() => {
  if (currentFilter.value === 'active') return tasks.value.filter(t => !t.status)
  if (currentFilter.value === 'completed') return tasks.value.filter(t => t.status)
  return tasks.value
})

// Format Tanggal
const todayFormatted = computed(() => {
  const options = { weekday: 'short', day: 'numeric', month: 'short' }
  return new Date().toLocaleDateString('id-ID', options)
})
</script>

<template>
  <div class="min-h-screen bg-[#FFF5F3] p-4 font-sans text-gray-700 md:p-8">
    <div class="mx-auto max-w-4xl space-y-4">
      
      <!-- BARIS 1: Header Banner & Card Progress (Bento Top) -->
      <div class="grid grid-cols-1 gap-4 md:grid-cols-3">
        
        <!-- Header Banner Warm Pink -->
        <div class="relative overflow-hidden rounded-3xl bg-gradient-to-r from-[#FF8A8A] to-[#FF6B6B] p-6 text-white shadow-sm md:col-span-2">
          <div class="flex items-center justify-between">
            <span class="inline-flex items-center gap-1 rounded-full bg-white/20 px-3 py-1 text-xs font-semibold backdrop-blur-sm">
              🐾 WARM & CUTE TASKIFY
            </span>
            <span class="inline-flex items-center gap-1 rounded-full bg-white/20 px-3 py-1 text-xs font-semibold backdrop-blur-sm">
              📅 {{ todayFormatted }}
            </span>
          </div>

          <h1 class="mt-4 text-3xl font-extrabold tracking-tight md:text-4xl">
            Taskify Vue ✨
          </h1>
          <p class="mt-2 max-w-sm text-sm font-medium text-red-50/90">
            Semangat jalani harimu! Catat semua tugas manismu di sini dan selesaikan satu per satu ya! 🎀
          </p>

          <!-- Hiasan bunga transparan -->
          <div class="absolute -bottom-6 -right-6 text-8xl opacity-15">🌸</div>
        </div>

        <!-- Card Progress Hari Ini -->
        <div class="flex flex-col justify-between rounded-3xl bg-white p-6 shadow-sm border border-rose-100/60">
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold uppercase tracking-wider text-rose-400">
              ❤️ PROGRESS HARI INI
            </span>
            <span class="text-xl">🐱</span>
          </div>

          <div class="my-3">
            <div class="text-4xl font-extrabold text-slate-800">
              {{ progressPercentage }}%
            </div>
            <p class="mt-1 text-xs font-medium text-rose-400">
              {{ progressPercentage === 100 ? 'Luar biasa! Semua tugas selesai 🎉' : 'Hebat banget! Tinggal sedikit lagi 🐾' }}
            </p>
          </div>

          <!-- Progress Bar -->
          <div class="h-3 w-full overflow-hidden rounded-full bg-rose-100/80">
            <div 
              class="h-full rounded-full bg-gradient-to-r from-rose-400 to-red-400 transition-all duration-500"
              :style="{ width: `${progressPercentage}%` }"
            ></div>
          </div>
        </div>

      </div>

      <!-- BARIS 2: Form Input Tugas (Bento Form) -->
      <div class="rounded-3xl bg-white p-4 shadow-sm border border-rose-100/60 md:p-6">
        <form @submit.prevent="addTask" class="flex flex-col gap-3 md:flex-row md:items-center">
          <!-- Input Teks -->
          <div class="relative flex-1">
            <span class="absolute inset-y-0 left-4 flex items-center text-rose-400">🪄</span>
            <input
              v-model="inputTask"
              type="text"
              placeholder="Tulis tugas manismu di sini... 🌸"
              class="w-full rounded-2xl border border-rose-200/80 bg-rose-50/30 py-3.5 pl-11 pr-4 text-sm font-medium text-gray-700 placeholder-rose-300 outline-none transition focus:border-rose-400 focus:bg-white focus:ring-2 focus:ring-rose-200"
            />
          </div>

          <!-- Select Prioritas -->
          <select 
            v-model="selectedPriority"
            class="rounded-2xl border border-rose-200/80 bg-rose-50/30 px-4 py-3.5 text-sm font-medium text-gray-600 outline-none transition focus:border-rose-400 focus:bg-white"
          >
            <option value="Penting! 💖">Penting! 💖</option>
            <option value="Santai 🌿">Santai 🌿</option>
          </select>

          <!-- Tombol Tambah -->
          <button
            type="submit"
            class="inline-flex items-center justify-center gap-2 rounded-2xl bg-gradient-to-r from-[#FF7575] to-[#FF5252] px-7 py-3.5 font-bold text-white shadow-md shadow-rose-200 transition hover:scale-[1.02] active:scale-95"
          >
            <span>+</span> Tambah ✨
          </button>
        </form>
      </div>

      <!-- BARIS 3: Filter & Daftar Tugas (Bento Content) -->
      <div class="grid grid-cols-1 gap-4 md:grid-cols-3">
        
        <!-- Filter Card (Samping Kiri) -->
        <div class="flex flex-col gap-2 rounded-3xl bg-white p-5 shadow-sm border border-rose-100/60">
          <h3 class="mb-2 text-xs font-bold uppercase tracking-wider text-rose-400">
            ⚙️ FILTER TUGAS
          </h3>
          
          <button 
            @click="currentFilter = 'all'"
            :class="currentFilter === 'all' ? 'bg-rose-100 text-rose-600 font-bold' : 'text-gray-500 hover:bg-rose-50'"
            class="w-full rounded-xl px-4 py-2.5 text-left text-sm transition"
          >
            📌 Semua ({{ tasks.length }})
          </button>
          
          <button 
            @click="currentFilter = 'active'"
            :class="currentFilter === 'active' ? 'bg-rose-100 text-rose-600 font-bold' : 'text-gray-500 hover:bg-rose-50'"
            class="w-full rounded-xl px-4 py-2.5 text-left text-sm transition"
          >
            ⏳ Belum Selesai ({{ tasks.filter(t => !t.status).length }})
          </button>
          
          <button 
            @click="currentFilter = 'completed'"
            :class="currentFilter === 'completed' ? 'bg-rose-100 text-rose-600 font-bold' : 'text-gray-500 hover:bg-rose-50'"
            class="w-full rounded-xl px-4 py-2.5 text-left text-sm transition"
          >
            ✅ Selesai ({{ completedCount }})
          </button>
        </div>

        <!-- Lista Tugas (Samping Kanan) -->
        <div class="rounded-3xl bg-white p-5 shadow-sm border border-rose-100/60 md:col-span-2">
          <div class="mb-4 flex items-center justify-between">
            <h2 class="font-bold text-gray-800">📋 Daftar Tugas Manis</h2>
            <button 
              v-if="completedCount > 0"
              @click="clearCompleted"
              class="text-xs font-medium text-rose-400 hover:underline"
            >
              Bersihkan Selesai
            </button>
          </div>

          <!-- Empty State -->
          <div v-if="filteredTasks.length === 0" class="py-8 text-center text-rose-300">
            <p class="text-3xl">🌸</p>
            <p class="mt-2 text-sm font-medium">Belum ada tugas di daftar ini!</p>
          </div>

          <!-- Item List -->
          <ul class="space-y-2.5">
            <li
              v-for="task in filteredTasks"
              :key="task.id"
              class="group flex items-center justify-between rounded-2xl bg-rose-50/40 p-3.5 transition hover:bg-rose-50/80"
            >
              <div class="flex items-center gap-3">
                <input
                  v-model="task.status"
                  type="checkbox"
                  class="h-5 w-5 rounded-md border-rose-300 text-rose-500 focus:ring-rose-300 cursor-pointer accent-rose-500"
                />
                <span
                  class="text-sm font-medium transition"
                  :class="task.status ? 'text-gray-400 line-through' : 'text-gray-700'"
                >
                  {{ task.title }}
                </span>
              </div>

              <div class="flex items-center gap-2">
                <span class="rounded-full bg-white px-2.5 py-1 text-[11px] font-medium text-rose-500 shadow-sm">
                  {{ task.priority }}
                </span>
                <button
                  @click="deleteTask(task.id)"
                  class="p-1 text-gray-300 transition hover:text-rose-500"
                  title="Hapus"
                >
                  🗑️
                </button>
              </div>
            </li>
          </ul>
        </div>

      </div>

    </div>
  </div>
</template>