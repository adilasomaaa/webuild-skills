# WeBuild Content Plugin

Plugin bundle untuk mengelola Artikel, Berita, dan Media di CMS **WeBuild** melalui AI agent. Di dalamnya sudah terbundel:

- **MCP Server** `webuild-content` (Streamable HTTP, via `mcp_config.json` & `.mcp.json`)
- **Skill** `/webuild-content` ([skills/webuild-content/SKILL.md](skills/webuild-content/SKILL.md))
- **Guard Rule** ([rules/AGENTS.md](rules/AGENTS.md)) agar mode coding dan mode penulisan CMS tetap terpisah

> **Catatan:** Plugin/skill ini **bersifat opsional**. MCP server `webuild-content` tetap bisa dipakai langsung tanpa plugin hanya dengan mendaftarkan endpoint URL + token Bearer di AI client Anda. Memasang plugin/skill ini memberikan alur `/webuild-content` (cek `whoami`, pilih aplikasi target, cek kategori/tag, draf otomatis) serta guard agar agent tidak sembarangan memanggil tool CMS saat sedang coding.

---

## 1. Prasyarat: Buat Token & Set Environment Variable

Buat token MCP di panel admin WeBuild (**Token MCP (Developer)**), lalu ekspor environment variable berikut di profil shell Anda (`~/.zshrc` atau `~/.bashrc`):

```bash
export WEBUILD_MCP_URL="https://webuild.example.com/mcp/content"
export WEBUILD_MCP_TOKEN="wbmcp_YOUR_TOKEN_HERE"
```

Muat ulang shell Anda:

```bash
source ~/.zshrc   # atau: source ~/.bashrc
```

> Untuk pengujian lokal, arahkan `WEBUILD_MCP_URL` ke `http://localhost:8001/mcp/content`.

---

## 2. Instalasi per AI Client

### Antigravity (Plugin Lengkap: MCP + Skill + Guard Rule)

1. Pastikan `WEBUILD_MCP_URL` dan `WEBUILD_MCP_TOKEN` sudah terekspor di shell Anda (restart Antigravity setelah mengubah `~/.zshrc` / `~/.bashrc` agar environment terbaca).
2. Pasang plugin `webuild-content` dari repo ini:

   **Opsi A — Menggunakan Antigravity CLI (`agy`):**
   ```bash
   git clone --depth 1 https://github.com/adilasomaaa/webuild-skills.git /tmp/webuild-skills
   agy plugin install /tmp/webuild-skills/plugins/webuild-content
   rm -rf /tmp/webuild-skills
   ```

   **Opsi B — Salin manual ke folder plugin Antigravity (tanpa CLI `agy`):**
   ```bash
   git clone --depth 1 https://github.com/adilasomaaa/webuild-skills.git /tmp/webuild-skills
   mkdir -p ~/.gemini/config/plugins
   cp -R /tmp/webuild-skills/plugins/webuild-content ~/.gemini/config/plugins/webuild-content
   rm -rf /tmp/webuild-skills
   ```

3. Verifikasi di Antigravity:
   - Pastikan plugin **WeBuild Content** muncul dan aktif di **Customizations > Plugins** (atau jalankan `agy plugin list`).
   - Pastikan server `webuild-content` tampil di **Additional Options (...) > MCP Servers**.
   - Jika sebelumnya Anda pernah mendaftarkan `webuild-content` secara manual di `~/.gemini/config/mcp_config.json`, hapus entry manual tersebut agar tidak duplikat.
   - Mulai percakapan baru lalu ketik `/webuild-content`.

> **Alternatif tanpa plugin (MCP saja di Antigravity):** tambahkan blok `webuild-content` langsung ke `~/.gemini/config/mcp_config.json` menggunakan URL dan token Bearer Anda.

---

### Claude Code (Plugin Lengkap: MCP + Skill + Guard Rule)

1. Pastikan `WEBUILD_MCP_URL` dan `WEBUILD_MCP_TOKEN` sudah terekspor di shell Anda.
2. Di dalam sesi Claude Code, jalankan:
   ```text
   /plugin marketplace add adilasomaaa/webuild-skills
   /plugin install webuild-content@webuild-skills
   ```
3. Mulai sesi baru dan jalankan `/webuild-content`.

> **Alternatif tanpa plugin (MCP saja di Claude Code):**
> ```bash
> claude mcp add --transport http webuild-content "$WEBUILD_MCP_URL" \
>   --header "Authorization: Bearer $WEBUILD_MCP_TOKEN"
> ```

---

### OpenAI Codex CLI (MCP + Skill `/webuild-content`)

Codex CLI membaca konfigurasi MCP dari `~/.codex/config.toml` (atau `.codex/config.toml` per proyek) dan skill dari `~/.agents/skills/`.

1. Tambahkan server MCP di `~/.codex/config.toml`:
   ```toml
   [mcp_servers.webuild-content]
   url = "https://webuild.example.com/mcp/content"
   bearer_token_env_var = "WEBUILD_MCP_TOKEN"
   ```
2. Pastikan `WEBUILD_MCP_TOKEN` sudah terekspor di shell (`~/.zshrc` / `~/.bashrc`):
   ```bash
   export WEBUILD_MCP_TOKEN="wbmcp_YOUR_TOKEN_HERE"
   ```
3. *(Opsional tapi disarankan)* Pasang skill `/webuild-content` untuk Codex:
   ```bash
   npx skills add adilasomaaa/webuild-skills --skill webuild-content -a codex -g -y
   ```
4. Jalankan `codex`, periksa koneksi dengan `/mcp`, lalu ketik `/webuild-content` (atau minta Codex menjalankan tool `whoami`).

---

### Gemini CLI (MCP + Skill `/webuild-content`)

1. Tambahkan server MCP di `~/.gemini/settings.json`:
   ```json
   {
     "mcpServers": {
       "webuild-content": {
         "httpUrl": "https://webuild.example.com/mcp/content",
         "headers": {
           "Authorization": "Bearer ${WEBUILD_MCP_TOKEN}"
         }
       }
     }
   }
   ```
2. *(Opsional tapi disarankan)* Pasang skill `/webuild-content`:
   ```bash
   npx skills add adilasomaaa/webuild-skills --skill webuild-content -a gemini-cli -g -y
   ```

---

## 3. Penggunaan Cepat

- `/webuild-content` — verifikasi koneksi (`whoami`), pilih aplikasi target, dan aktifkan mode penulisan konten.
- `/webuild-content off` — keluar dari mode CMS dan kembali ke mode coding normal.
