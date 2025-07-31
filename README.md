# Spesifikasi Fungsional & Panduan Interaksi UI AL 'ILLM

**Tujuan Dokumen:** Dokumen ini berfungsi sebagai panduan teknis bagi desainer UI/UX dan developer frontend untuk memahami, mereplikasi, dan mengembangkan fungsionalitas yang dirancang dalam mockup `index.html`.

**Filosofi Inti:** Setiap elemen dan interaksi bertujuan untuk mewujudkan tiga prinsip utama: **Otoritas yang Transparan**, **Eksplorasi Terpandu**, dan **Kejelasan Intuitif**.

---

## 1. Struktur Layout & Komponen Utama

Aplikasi menggunakan layout tiga kolom yang fleksibel:
1.  **Sidebar Kiri (`.sidebar`):** Berisi profil pengguna, aksi utama, dan riwayat percakapan. Dapat diciutkan (*collapsible*).
2.  **Konten Utama (`.main-content`):** Area dinamis untuk menampilkan alur percakapan.
3.  **Sidebar Kanan (`.sidebar-right`):** Berisi menu navigasi utama untuk laman-laman proyek. Dapat diciutkan (*collapsible*).

### 1.1. Sidebar Kiri (`<aside class="sidebar" id="sidebar-left">`)

Sidebar kiri berfungsi sebagai pusat kontrol percakapan dan identitas pengguna. Terdiri dari dua zona fungsional utama.

- **Zona 1: Profil & Aksi Utama**
    - **Elemen:**
        1.  `profile-card`: Menampilkan avatar, nama, dan kelas pengguna.
        2.  `credits`: Menampilkan sisa kredit.
        3.  `new-chat-btn`: Tombol "Chat Baru".
    - **Spesifikasi Interaksi:**
        - **Klik `new-chat-btn`:** Membersihkan seluruh konten di area `.chat-messages` dan mempersiapkan state untuk percakapan baru. Riwayat chat sebelumnya harus diarsipkan.
        - **Tooltip:** Saat hover di atas tombol "Chat Baru", muncul tooltip "Mulai percakapan baru".

- **Zona 2: Riwayat Chat (`.chat-history`)**
    - **Elemen:** Daftar tautan percakapan sebelumnya.
    - **Spesifikasi Interaksi:**
        - **Klik Item Riwayat:** Memuat ulang seluruh state percakapan yang dipilih ke dalam area `.chat-messages`, termasuk semua pertanyaan pengguna dan jawaban bot yang terkait.
        - **Tooltip:** Setiap item riwayat memiliki tooltip yang mengklarifikasi aksinya (contoh: "Lanjutkan percakapan tentang 'Hari Santri'").

### 1.2. Sidebar Kanan (`<aside class="sidebar-right" id="sidebar-right">`)

Sidebar kanan didedikasikan untuk navigasi antar-laman utama proyek.

- **Elemen:** `.main-menu` yang berisi daftar tautan (`<a>`) pilar-pilar proyek.
- **Spesifikasi Interaksi:**
    - **Klik Item Menu:** Mengubah tampilan di area `.main-content`. Kontainer `.chat-container` akan disembunyikan atau diganti dengan konten yang relevan dengan menu yang dipilih (misalnya, visualisasi Knowledge Graph atau halaman teks statis tentang metodologi). Item menu yang diklik diberi kelas `.active`.
    - **Tooltip:** Setiap item menu memiliki tooltip yang menjelaskan halaman tujuan (contoh: "Jelajahi struktur data dan hubungan entitas" untuk menu Knowledge Graph).

### 1.3. Konten Utama (`<main class="main-content">`)

- **Elemen:** Terdiri dari `.chat-header` dan `.chat-container`.
- **Spesifikasi Interaksi:** Lebar area ini secara otomatis menyesuaikan saat salah satu atau kedua sidebar di-expand/collapse untuk mengisi ruang yang tersedia.

- **Header (`.chat-header`)**
    - **Elemen:** Dua tombol *toggle* dan logo.
    - **Spesifikasi Interaksi:**
        - **Klik `#menu-toggle-left`:** Memicu fungsi JavaScript untuk menambahkan/menghapus kelas `.collapsed` pada elemen `#sidebar-left`, menciptakan efek geser yang mulus.
        - **Klik `#menu-toggle-right`:** Memicu fungsi JavaScript untuk menambahkan/menghapus kelas `.collapsed` pada elemen `#sidebar-right`.
        - **Tooltip:** Setiap tombol menu menampilkan tooltip yang sesuai ("Buka/Tutup Sidebar Kiri" dan "Buka/Tutup Menu Laman").

---

## 2. Alur & Mekanisme Percakapan (`.chat-messages`)

Ini adalah komponen paling dinamis dan krusial.

### 2.1. Kartu Respons Bot (`.message-content`)

Setiap jawaban dari bot harus berisi komponen-komponen berikut untuk memastikan transparansi dan fungsionalitas.

- **Status Jawaban (`.status-badge`)**
    - **Fungsi:** Indikator visual paling utama. Harus langsung terlihat. `Jawaban Sahih` untuk konten dari Q&A pairs tervalidasi; `Jawaban Belum Ditashih` untuk konten yang di-generate oleh model AI.

- **Sumber Informasi (`.source-section`)**
    - **Elemen:** Terdiri dari dua sub-seksi: "Entitas Knowledge Graph" dan "Dokumen Teranotasi". Keduanya berisi elemen `.source-chip`.
    - **Spesifikasi Interaksi:**
        - **Ekstraksi Dinamis & Tampilan Penuh:** Semua chip yang relevan dengan jawaban harus diekstrak dan ditampilkan. Jika jumlah chip melebihi lebar kontainer, mereka akan secara otomatis mengalir ke baris berikutnya (`flex-wrap: wrap`) untuk memastikan tidak ada sumber yang tersembunyi.
        - **Klik `.source-chip`:** Memicu event untuk menampilkan informasi detail tentang entitas atau dokumen tersebut. Ini bisa berupa *overlay*, *modal*, atau tampilan baru di sidebar. Contoh: mengklik chip `Imam Syafi'i` akan menampilkan node-nya di Knowledge Graph.
        - **Tooltip:** Setiap chip memiliki tooltip yang menjelaskan aksinya (contoh: "Lihat entitas 'Imam Syafi'i' dalam Knowledge Graph").

### 2.2. Riset Lanjutan (`.research-suggestions`)

- **Fungsi:** Komponen proaktif untuk memandu pengguna.
- **Spesifikasi Interaksi:**
    - **Klik Pertanyaan:** Saat pengguna mengklik sebuah tautan pertanyaan di daftar ini:
        1.  Tautan tersebut harus secara visual ditandai sebagai "sudah ditanyakan". Mockup ini menggunakan kelas `.answered` yang membuatnya berwarna abu-abu dan menambahkan tag `<span>Terjawab</span>` di sebelahnya. Tautan yang sudah dijawab menjadi non-interaktif (`pointer-events: none`).
        2.  Teks dari pertanyaan yang diklik harus secara otomatis dikirim sebagai input pengguna baru, memicu giliran percakapan selanjutnya.

### 2.3. Input Pengguna (`.chat-input-area-wrapper`)

- **Elemen:** `input` field dan tombol kirim.
- **Spesifikasi Interaksi:**
    - **Klik Tombol Kirim:** Memicu pengiriman konten dari input field.
    - **Tooltip:** Hover di atas tombol kirim menampilkan "Kirim Pesan".

Dengan mengikuti spesifikasi ini, tim pengembangan dapat membangun antarmuka yang fungsional dan setia pada visi konseptual proyek AL 'ILLM.