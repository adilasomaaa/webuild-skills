---
name: webuild-content
description: >-
  Inisialisasi dan aktifkan koneksi MCP `webuild-content` untuk membuat, menyunting,
  menerbitkan, mengarsipkan, atau menghapus Artikel, Berita, dan Media di CMS WeBuild.
  Gunakan skill ini ketika pengguna memanggil `/webuild-content`, meminta mengecek
  koneksi MCP WeBuild (`/me` / `whoami`), atau mengaktifkan/menonaktifkan mode penulisan
  konten WeBuild sepanjang sesi.
---

# WeBuild Content MCP (`/webuild-content`)

Skill ini menjadi gerbang inisialisasi (*init*) untuk memastikan MCP server `webuild-content` benar-benar terhubung, target aplikasi sudah tepat, dan memisahkan sesi penulisan berita/artikel dari sesi coding biasa.

## 1. Perintah Keluar Mode (`/webuild-content off`)

Jika pengguna menulis `/webuild-content off`, *"keluar mode webuild"*, atau *"kembali ke mode coding"*:
- Nonaktifkan mode sesi WeBuild untuk percakapan ini.
- Konfirmasi singkat bahwa percakapan kembali ke **mode coding normal** dan tool MCP `webuild-content` tidak akan dipanggil lagi kecuali diaktifkan ulang dengan `/webuild-content`.

## 2. Langkah Inisialisasi (Wajib saat Pertama Dipicu)

1. **Verifikasi koneksi MCP**: Panggil tool MCP `whoami` dari server `webuild-content`.
   - **Jika tool tidak ditemukan atau gagal terhubung** (`401`, `403`, connection refused):
     - Nyatakan dengan jelas bahwa **MCP `webuild-content` belum terhubung**.
     - Berikan langkah perbaikan singkat:
       1. Pastikan environment variable `WEBUILD_MCP_URL` (mis. `https://domain-webuild.anda/mcp/content` atau `http://localhost:8001/mcp/content`) dan `WEBUILD_MCP_TOKEN` (`wbmcp_...`) sudah terpasang di environment shell/sistem.
       2. Pastikan server `webuild-app` aktif dan dapat diakses di `WEBUILD_MCP_URL`.
       3. Pastikan token belum kedaluwarsa/dicabut, akun memiliki status **Developer** (`is_developer = true`), dan plugin `webuild-content` dalam keadaan aktif.
     - Hentikan proses hingga koneksi berhasil.
   - **Jika berhasil**: Tampilkan ringkasan status **Siap Menulis di WeBuild**:
     - **Status**: Terhubung (`WeBuild Content MCP`)
     - **Pengguna**: `<name>` (`<email>`) — role `<roles>`
     - **Target Aplikasi Aktif**: `<nama aplikasi>` (`<hostname>`) atau *(belum dipilih)*
     - **Izin Token (Abilities)**: tampilkan ringkas apakah bisa buat draft (`content.write`), publikasi (`content.publish`), hapus (`content.delete`), dan unggah media (`media.write`).

2. **Pastikan Target Aplikasi** (jika `active_target_application` masih `null`):
   - Tampilkan daftar `accessible_applications` dari hasil `whoami` (atau panggil `list_applications`).
   - Minta pengguna memilih aplikasi target, lalu jalankan `set_target_application` agar seluruh perintah berikutnya konsisten pada aplikasi tersebut.

3. **Pilih Cakupan Sesi (Ingat Sepanjang Sesi vs Sekali Pakai)**:
   - Jika belum ditetapkan dalam sesi ini, tanyakan ke pengguna (gunakan `ask_question` jika tersedia, atau pertanyaan pilihan singkat):
     - **(Recommended) Ingat sepanjang sesi ini** — semua pesan berikutnya diarahkan ke MCP `webuild-content` hingga pengguna mengetik `/webuild-content off` atau *"keluar mode webuild"*.
     - **Sekali ini saja** — hanya untuk permintaan ini, lalu otomatis kembali ke mode coding normal.
   - Jika pengguna memanggil `/webuild-content <tugas langsung>` (mis. `/webuild-content buatkan draft berita ...`), selesaikan dulu pengecekan `whoami` dan tugasnya, lalu tanyakan apakah ingin mengaktifkan mode WeBuild untuk sisa sesi ini.

## 3. Aturan Kerja Selama Mode WeBuild Aktif

- Selalu periksa kategori dan tag yang sudah ada lewat `list_categories_and_tags` sebelum membuat konten.
- Untuk gambar utama, gunakan `list_media` atau `upload_media` lalu isi `featured_image_media_id`. Jangan pernah menyisipkan `data:image/...;base64,` di dalam HTML `content`.
- `create_content` **selalu** menghasilkan status `draft`.
- **Jangan pernah** memanggil `publish_content`, `archive_content`, atau `delete_content` tanpa permintaan eksplisit dari pengguna.
- Setelah selesai membuat atau memperbarui konten, tampilkan ringkasan: **Judul**, **Jenis** (Berita/Artikel), **Aplikasi Target**, **Status** (`draft`/`terbit`/`arsip`), dan **ID Konten**.
