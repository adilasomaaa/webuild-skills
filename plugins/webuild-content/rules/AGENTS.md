# WeBuild Content MCP Guard

Plugin `webuild-content` menyediakan MCP server `webuild-content` untuk mengelola Artikel, Berita, dan Media di CMS WeBuild.

Aturan pemisahan mode coding vs CMS WeBuild:
1. **Default adalah mode coding**: Pada prompt biasa (termasuk saat membahas fitur berita, artikel, kategori, atau media di kode sumber), **jangan memanggil** tool MCP `webuild-content` secara otomatis.
2. **Pemicu eksplisit**: Hanya panggil tool MCP `webuild-content` jika:
   - Pengguna menginisialisasi atau memanggil `/webuild-content` (skill `webuild-content`), atau
   - Pengguna secara eksplisit meminta operasi pada CMS WeBuild / MCP `webuild-content`, atau
   - Pengguna telah memilih untuk mengaktifkan **mode WeBuild sepanjang sesi ini** (berakhir saat pengguna mengetik `/webuild-content off` atau *"keluar mode webuild"*).
3. **Konfirmasi tindakan berisiko**: Meskipun mode WeBuild sedang aktif, jangan pernah menjalankan `publish_content`, `archive_content`, atau `delete_content` tanpa instruksi eksplisit dari pengguna.
