# Reduksi Beban Kognitif (HCI) pada Dashboard Peringatan Dini Bencana

## Deskripsi

Proyek ini merupakan purwarupa dan studi kasus penerapan Interaksi Manusia dan Komputer (IMK) dalam merancang antarmuka aplikasi *mission-critical*.
Proyek ini mengadaptasi materi praktisi mengenai prinsip psikologi kognitif dan evaluasi UI/UX, dengan fokus khusus pada eliminasi *chartjunk* (hiasan visual berlebih) dan manajemen *Extraneous Load* (beban mental tambahan) yang dapat mengganggu pengambilan keputusan dalam situasi bertekanan tinggi.

## Tujuan

Tujuan proyek ini adalah:
1. Merancang arsitektur antarmuka sistem peringatan dini bencana yang bebas dari elemen visual yang membingungkan.
2. Mengimplementasikan prinsip hierarki informasi: *"Overview first, zoom and filter, then detail on demand"* menggunakan ekosistem *frontend* modern (Vue 3 dan Tailwind CSS).
3. Mengevaluasi kebergunaan desain melalui parameter pencegahan kesalahan (*Error Prevention*) dan visibilitas status sistem (*Visibility of System Status*) berdasarkan Heuristik Jacob Nielsen.
4. Menyimulasikan pergantian *state* kedaruratan (*Aman* ke *Bahaya*) secara reaktif untuk menguji efisiensi komunikasi visual antarmuka.

## Studi Kasus

Operator di pusat komando mitigasi bencana saat ini memantau aliran data dari puluhan sensor alam (seismograf, peringatan dini tsunami, sensor angin) secara bersamaan. *Dashboard* konvensional menampilkan seluruh metrik teknis dan grafik fluktuasi secara bertumpuk di satu layar utama. 
Pada saat terjadi gempa bumi (detik-detik kritis), lonjakan grafik yang berkedip dan data yang tumpang tindih ini menyebabkan *Extraneous Load* pada otak operator, memicu kelumpuhan analisis (*analysis paralysis*), dan memperlambat waktu respons aktivasi sirine evakuasi.
Melalui arsitektur UI baru yang diusulkan ini, layar utama hanya akan menampilkan satu kesimpulan status makro (Aman/Waspada/Bahaya). Metrik sensor mentah (angka *Richter Scale*, tekanan barometer) akan disembunyikan menggunakan prinsip *Detail on Demand*, dan hanya dimunculkan secara instan jika sistem mendeteksi anomali.

## Metodologi Antarmuka

Arsitektur antarmuka dibangun dengan panduan psikologi kognitif:
1. **Gestalt Principles:** Mengelompokkan area peringatan secara terpisah dari area rincian metrik teknis.
2. **Utilitarian Design:** Memanfaatkan ruang kosong (*white space*) dan membuang garis batas tabel yang tebal (mengikuti prinsip minimalisme Edward Tufte).
3. **Reactivity & State Management:** Menggunakan *binding* status reaktif (Vue.js) agar warna layar (*green/red*) dan tombol aksi kritis (*Trigger Siren*) berubah seketika tanpa perlu me-*refresh* halaman browser.

## Evaluasi

Berbeda dengan proyek analitika data, evaluasi proyek ini didasarkan pada metrik interaksi (IMK):
*   *Time to Decision:* (Simulasi) Kecepatan operator memahami status utama dalam pandangan pertama (direpresentasikan oleh ukuran *banner* status yang dominan).
*   *Error Prevention Rate:* Pencegahan *misclick* melalui penyembunyian tombol sirine saat kondisi aman, dan penambahan layer konfirmasi (diwakili fungsi *Alert*) saat kondisi darurat.

## Teknologi

*   Vue 3 (Composition API / CDN Concept)
*   Tailwind CSS (Utility-First Styling)
*   HTML/JS Murni (Untuk kemudahan eksekusi tanpa instalasi NodeJS bagi evaluator)

## Cara Menjalankan

Karena ini berfokus pada antarmuka *frontend*, proyek ini tidak memerlukan Google Colab.
1. Unduh file `emergency_dashboard_hci.html`.
2. Buka file tersebut langsung menggunakan peramban web modern apa pun (Chrome, Edge, Firefox). File ini memuat Vue.js dan Tailwind secara langsung dari *Content Delivery Network* (CDN).
3. Uji coba antarmuka dengan menekan tombol simulasi peringatan darurat di bagian bawah untuk melihat pergantian *state* kognitif.

## Kesimpulan
Proyek ini mendemonstrasikan bahwa dalam perancangan perangkat lunak untuk keselamatan publik, "desain yang baik" bukanlah desain yang cantik, melainkan desain yang paling cepat dipahami oleh otak manusia di bawah tekanan.
Penggunaan kerangka kerja reaktif seperti Vue.js yang dipadukan dengan Tailwind CSS memungkinkan arsitek sistem membangun dashboard yang fungsional, bersih, dan mematuhi hukum Interaksi Manusia dan Komputer tanpa harus berkompromi pada performa penguraian data real-time.


## Struktur Repository

```text
sesi-5-hci/
├── emergency_dashboard_hci.html
├── README.md
└── requirements.txt
