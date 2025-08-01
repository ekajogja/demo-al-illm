# Spesifikasi Fungsional & Panduan Interaksi UI AL 'ILLM

**Tujuan Dokumen:** Dokumen ini berfungsi sebagai panduan teknis bagi desainer UI/UX dan developer frontend untuk memahami, mereplikasi, dan mengembangkan fungsionalitas yang dirancang dalam mockup `index.html`.

**Filosofi Inti:** Setiap elemen dan interaksi bertujuan untuk mewujudkan tiga prinsip utama: **Otoritas yang Transparan**, **Eksplorasi Terpandu**, dan **Kejelasan Intuitif**.

**Pembaruan Terakhir:** Fungsionalitas telah diperbarui secara signifikan untuk menyertakan perilaku responsif dan interaksi berbasis modal. `index.html` kini merupakan file mandiri (*self-contained*) yang menyematkan semua CSS, JavaScript, dan konten teks yang diperlukan.

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
    - **Klik Item Menu:** **Membuka jendela modal** yang menampilkan konten penjelasan yang relevan. Konten ini disematkan langsung di dalam file `index.html` dari dokumen `esai-final.md`.
    - **Tooltip:** Setiap item menu memiliki tooltip yang menjelaskan halaman tujuan (contoh: "Jelajahi struktur data dan hubungan entitas" untuk menu Knowledge Graph).

### 1.3. Konten Utama (`<main class="main-content">`)

- **Elemen:** Terdiri dari `.chat-header` dan `.chat-container`.
- **Spesifikasi Interaksi:** Lebar area ini secara otomatis menyesuaikan saat salah satu atau kedua sidebar di-expand/collapse untuk mengisi ruang yang tersedia.

- **Header (`.chat-header`)**
    - **Elemen:** Dua tombol *toggle* dan logo.
    - **Spesifikasi Interaksi:**
        - **Klik `#menu-toggle-left`:** Memicu fungsi JavaScript untuk menambahkan/menghapus kelas `.collapsed` pada elemen `#sidebar-left`.
        - **Klik `#menu-toggle-right`:** Memicu fungsi JavaScript untuk menambahkan/menghapus kelas `.collapsed` pada elemen `#sidebar-right`.
        - **Tooltip:** Setiap tombol menu menampilkan tooltip yang sesuai ("Buka/Tutup Sidebar Kiri" dan "Buka/Tutup Menu Laman").

---

## 2. Alur & Mekanisme Percakapan (`.chat-messages`)

Ini adalah komponen paling dinamis dan krusial.

### 2.1. Kartu Respons Bot (`.message-content`)

Setiap jawaban dari bot harus berisi komponen-komponen berikut untuk memastikan transparansi dan fungsionalitas.

- **Status Jawaban (`.status-badge`)**
    - **Fungsi:** Indikator visual paling utama. Harus langsung terlihat. `Jawaban Sahih` untuk konten dari Q&A pairs tervalidasi; `Jawaban Belum Ditashih` untuk konten yang di-generate oleh model AI.

- **Metadata Jawaban (`.metadata`)**
    - **Elemen:** Menampilkan **Kontributor Q&A** dan **Mushahih**.
    - **Spesifikasi Interaksi:** Nama kontributor dan mushahih kini berupa **tautan yang dapat diklik**. Mengklik tautan ini akan **membuka jendela modal** yang menampilkan profil singkat dari individu atau lembaga tersebut.

- **Sumber Informasi (`.source-section`)**
    - **Elemen:** Terdiri dari dua sub-seksi: "Entitas Knowledge Graph" dan "Dokumen Teranotasi". Keduanya berisi elemen `.source-chip`.
    - **Spesifikasi Interaksi:**
        - **Klik `.source-chip`:** **Membuka jendela modal yang saat ini hanya menampilkan judul dari sumber tersebut sebagai placeholder.** Konten detail untuk setiap *chip* akan diimplementasikan di masa mendatang (lihat Daftar Tugas).
        - **Tooltip:** Setiap chip memiliki tooltip yang menjelaskan aksinya (contoh: "Lihat entitas 'Imam Syafi'i' dalam Knowledge Graph").

### 2.2. Riset Lanjutan (`.research-suggestions`)

- **Fungsi:** Komponen proaktif untuk memandu pengguna.
- **Spesifikasi Interaksi:**
    - **Klik Pertanyaan:** Saat pengguna mengklik sebuah tautan pertanyaan di daftar ini, teks dari pertanyaan tersebut harus secara otomatis dikirim sebagai input pengguna baru, memicu giliran percakapan selanjutnya.

### 2.3. Input Pengguna (`.chat-input-area-wrapper`)

- **Elemen:** `input` field dan tombol kirim.
- **Spesifikasi Interaksi:**
    - **Klik Tombol Kirim:** Memicu pengiriman konten dari input field.
    - **Tooltip:** Hover di atas tombol kirim menampilkan "Kirim Pesan".

---

## 3. Perilaku Responsif

Aplikasi ini dirancang untuk menjadi *mobile-friendly* dengan logika sebagai berikut:

- **Layar Besar (Lebar > 992px):** Kedua sidebar (kiri dan kanan) akan terlihat secara default.
- **Layar Kecil (Lebar < 992px):** Kedua sidebar akan **otomatis diciutkan** (*collapsed*) saat halaman dimuat untuk memaksimalkan area konten.
- **Mekanisme "Push Content":** Di layar kecil, saat salah satu sidebar dibuka, ia akan **mendorong** konten utama ke samping, bukan menindihnya. Ini memastikan tombol *toggle* di header tetap terlihat dan dapat diakses oleh pengguna.

---

## 4. Fungsionalitas Modal Knowledge Graph

Setiap `.source-chip` di dalam aplikasi kini bersifat interaktif. Saat diklik, sebuah jendela modal akan muncul, menampilkan kartu profil detail untuk entitas yang dipilih. Konten ini dimuat secara dinamis dari objek `sourceChipContent` yang ada di `index.html`. Struktur data dan relasi di dalam objek ini secara ketat mematuhi `skema-knowledge-graph.md` untuk memastikan integritas dan konsistensi data.

Fitur ini juga mendukung **penjelajahan rekursif**: mengklik tautan entitas lain di dalam modal akan memuat profil entitas baru tersebut di dalam modal yang sama, memungkinkan eksplorasi mendalam tanpa hambatan.

**Contoh Implementasi untuk Chip "Hari Santri":**
-   **HTML:** `<a href="#" class="source-chip" data-chip-id="hari-santri">Hari Santri</a>`
-   **JavaScript (dalam `sourceChipContent`):**
    ```javascript
    "hari-santri": {
        nama_entitas: "Hari Santri",
        kategori_entitas: "Geopolitik dan Sejarah Dunia / Peristiwa Sejarah Dunia",
        deskripsi: "Hari peringatan nasional di Indonesia yang ditetapkan pada tanggal 22 Oktober setiap tahunnya, untuk mengenang seruan Resolusi Jihad yang dicetuskan oleh Hadratussyaikh KH Hasyim Asy'ari pada 22 Oktober 1945.",
        tokoh_kunci: ["kh-hasyim-asyari"],
        konteks_historis: ["resolusi-jihad", "pertempuran-surabaya"],
        dasar_hukum: ["keppres-no-22-tahun-2015"],
        kontributor: ["tim-sejarah-uin-jakarta"],
        validator: ["iip-d-yahya"],
        diperbarui: "1 Agustus 2025",
        referensi: ["keppres-no-22-tahun-2015"]
    }
    ```
-   **Output Modal (dihasilkan oleh `showModal`):**
    ```html
    <h4>Kategori Entitas</h4>
    <p>Geopolitik dan Sejarah Dunia / Peristiwa Sejarah Dunia</p>
    <h4>Deskripsi</h4>
    <p>Hari peringatan nasional di Indonesia yang ditetapkan pada tanggal 22 Oktober setiap tahunnya, untuk mengenang seruan Resolusi Jihad yang dicetuskan oleh Hadratussyaikh KH Hasyim Asy'ari pada 22 Oktober 1945.</p>
    <h4>Tokoh Kunci</h4>
    <ul>
        <li><a href="#" data-chip-id="kh-hasyim-asyari">KH Hasyim Asy'ari</a></li>
    </ul>
    <h4>Konteks Historis</h4>
    <ul>
        <li><a href="#" data-chip-id="resolusi-jihad">Resolusi Jihad</a></li>
        <li><a href="#" data-chip-id="pertempuran-surabaya">Pertempuran Surabaya</a></li>
    </ul>
    <h4>Dasar Hukum</h4>
    <ul>
        <li><a href="#" data-chip-id="keppres-no-22-tahun-2015">Keppres No. 22 Tahun 2015</a></li>
    </ul>
    <div class="metadata-box">
        <div class="metadata-row"><strong>Kontributor:</strong> <span><a href="#" data-contributor-id="tim-sejarah-uin-jakarta">Tim Sejarah Peradaban Islam UIN Syarif Hidayatullah</a></span></div>
        <div class="metadata-row"><strong>Validator:</strong> <span><a href="#" data-contributor-id="iip-d-yahya">Iip D. Yahya</a></span></div>
        <div class="metadata-row"><strong>Diperbarui:</strong> <span>1 Agustus 2025</span></div>
        <div class="metadata-row"><strong>Referensi:</strong> <span><a href="#" data-chip-id="keppres-no-22-tahun-2015">Keppres No. 22 Tahun 2015</a></span></div>
    </div>
    ```

Dengan mengikuti spesifikasi ini, tim pengembangan dapat membangun antarmuka yang fungsional dan setia pada visi konseptual proyek AL 'ILLM.
