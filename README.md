# Sesi 5: Human-Computer Interaction (HCI) & Data Visualization
**Implementasi *Dashboard* Peringatan Dini Berbasis Reduksi Beban Kognitif**

## 📖 Deskripsi Studi Kasus
Dalam sistem berisiko tinggi seperti mitigasi bencana atau instalasi gawat darurat, kegagalan antarmuka dalam menyajikan informasi dapat berakibat fatal. *Dashboard* konvensional sering kali menampilkan seluruh metrik sensor secara bersamaan (banjir data / *chartjunk*), yang justru memicu kelumpuhan analisis (*analysis paralysis*) dan kelelahan mental (*extraneous load*) pada operator di detik-detik kritis.

Studi kasus ini mendemonstrasikan implementasi antarmuka *dashboard* berbasis web yang secara ketat mematuhi prinsip psikologi kognitif dan 10 Heuristik Jacob Nielsen. Antarmuka ini dibangun menggunakan kerangka kerja reaktif (Vue.js) dan dipoles dengan gaya utiliter (Tailwind CSS) untuk mengeksekusi mantra desain visual: *"Overview first, detail on demand"*.

## 🎯 Tujuan Eksekusi
1. **Reduksi Extraneous Load:** Menghilangkan elemen visual yang tidak esensial (seperti batas tabel yang tebal atau animasi berlebihan) yang dapat mendistraksi fokus operator.
2. **Implementasi *Overview First*:** Menyajikan status makro (Tingkat Bahaya) sebagai informasi utama yang mendominasi hierarki visual.
3. **Implementasi *Detail on Demand*:** Menyembunyikan metrik teknis sensor (seperti kecepatan milidetik seismograf atau tekanan air) di balik interaksi, sehingga layar tetap bersih namun data teknis tetap tersedia jika situasi menuntut eskalasi.
4. **Pencegahan Kesalahan (Error Prevention):** Menerapkan tombol aksi kritis dengan konfirmasi langkah ganda yang tertegas oleh palet warna (merah untuk aksi destruktif/darurat).

## 💻 Implementasi Kode (Vue 3 Composition API & Tailwind CSS)
Berikut adalah simulasi komponen antarmuka yang dapat dijalankan pada *file* komponen Vue tunggal `EmergencyDashboard.vue`.

```html
<template>
  <!-- Layout Utilitarian: Warna latar yang netral untuk mengurangi kelelahan mata -->
  <div class="min-h-screen bg-gray-50 p-8 font-sans">
    
    <header class="mb-8 border-b pb-4">
      <h1 class="text-2xl font-bold text-gray-800">Sistem Peringatan Dini Bencana Pusat</h1>
      <p class="text-sm text-gray-500">Live Status Monitor - Zona Pesisir Barat</p>
    </header>

    <!-- 1. OVERVIEW FIRST: Banner Status Makro -->
    <div 
      :class="statusBencana === 'BAHAYA' ? 'bg-red-100 border-red-500 text-red-800' : 'bg-green-50 border-green-400 text-green-800'"
      class="border-l-4 p-6 rounded-md mb-8 transition-colors duration-300 shadow-sm"
    >
      <div class="flex items-center justify-between">
        <div>
          <h2 class="text-lg font-semibold tracking-wide">STATUS SAAT INI: {{ statusBencana }}</h2>
          <p class="text-sm mt-1 opacity-90">{{ pesanStatus }}</p>
        </div>
        
        <!-- Tombol Aksi Kritis: Visibilitas Sistem (Visibility of System Status) -->
        <button 
          v-if="statusBencana === 'BAHAYA'"
          @click="toggleSiren"
          class="px-6 py-3 bg-red-600 hover:bg-red-700 text-white font-bold rounded shadow-lg flex items-center gap-2"
        >
          <span class="animate-pulse">⚠️</span> AKTIFKAN SIRINE EVAKUASI
        </button>
      </div>
    </div>

    <!-- 2. DETAIL ON DEMAND: Kontrol Akordion untuk Metrik Sensor -->
    <section class="bg-white rounded-lg border border-gray-200 overflow-hidden shadow-sm">
      <button 
        @click="showSensorDetails = !showSensorDetails"
        class="w-full flex justify-between items-center p-4 bg-gray-100 hover:bg-gray-200 transition focus:outline-none"
      >
        <span class="font-medium text-gray-700">🔍 Rincian Metrik Sensor (Detail on Demand)</span>
        <span class="text-gray-500">{{ showSensorDetails ? 'Tutup ▲' : 'Buka ▼' }}</span>
      </button>

      <div v-show="showSensorDetails" class="p-6 grid grid-cols-1 md:grid-cols-3 gap-6">
        <!-- Metrik tanpa chartjunk, langsung ke inti data -->
        <div class="border p-4 rounded bg-gray-50">
          <p class="text-xs text-gray-500 uppercase tracking-wider">Aktivitas Seismik</p>
          <p class="text-2xl font-bold text-gray-800 mt-1">4.2 <span class="text-sm font-normal text-gray-500">SR</span></p>
        </div>
        <div class="border p-4 rounded bg-gray-50">
          <p class="text-xs text-gray-500 uppercase tracking-wider">Muka Air Laut</p>
          <p class="text-2xl font-bold text-gray-800 mt-1">+1.5 <span class="text-sm font-normal text-gray-500">meter (Surut)</span></p>
        </div>
        <div class="border p-4 rounded bg-gray-50">
          <p class="text-xs text-gray-500 uppercase tracking-wider">Kecepatan Angin</p>
          <p class="text-2xl font-bold text-gray-800 mt-1">12 <span class="text-sm font-normal text-gray-500">km/h</span></p>
        </div>
      </div>
    </section>

    <!-- Fitur Simulasi untuk Pengujian Heuristik -->
    <div class="mt-12 pt-6 border-t border-gray-200 text-center">
      <p class="text-xs text-gray-400 mb-4">-- Panel Simulasi Pengujian --</p>
      <button 
        @click="simulasiGempa"
        class="px-4 py-2 bg-gray-800 text-white text-sm rounded hover:bg-gray-900"
      >
        Trigger Simulasi Darurat
      </button>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

// Reaktivitas State
const statusBencana = ref('AMAN')
const pesanStatus = ref('Seluruh metrik sensor berada dalam ambang batas normal.')
const showSensorDetails = ref(false)

// Logika Bisnis Interaktif
const simulasiGempa = () => {
  statusBencana.value = 'BAHAYA'
  pesanStatus.value = 'PERINGATAN: Anomali seismik tinggi terdeteksi. Segera evaluasi potensi tsunami.'
  // Sistem otomatis membuka rincian (Detail on Demand) saat darurat untuk mempercepat analisis operator
  showSensorDetails.value = true 
}

const toggleSiren = () => {
  alert('Sistem: Konfirmasi ulang! Apakah Anda yakin ingin memicu sirine evakuasi pesisir?')
  // Menerapkan prinsip Error Prevention (Pencegahan Kesalahan fatal)
}
</script>
