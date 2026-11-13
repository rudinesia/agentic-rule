# Kumpulan Prompt

Prompt siap pakai untuk AI agent: menganalisis codebase, mendokumentasikan modul, menyusun dan menjalankan rencana kerja, serta merapikan tulisan. Salin isi file ke chat agent, ganti placeholder `[...]`, lalu kirim.

## Daftar Isi

- [Daftar Prompt](#daftar-prompt)
- [Cara Pakai](#cara-pakai)
- [Detail Prompt](#detail-prompt)
- [Hubungan dengan Folder Lain](#hubungan-dengan-folder-lain)

## Daftar Prompt

| File | Kegunaan | Placeholder | Ukuran |
|------|----------|-------------|--------|
| [`codebase-analyzer.md`](codebase-analyzer.md) | Audit codebase menyeluruh (read-only) menjadi dokumentasi di `docs/_audit/` | tidak ada | Besar |
| [`modul-analyzer.md`](modul-analyzer.md) | Dokumentasi teknis satu modul di `docs/features/` | `[nama_modul]`, `[url_modul]`, `[alur_yang_ingin_diketahui]`, `[targeted_skill]` | Kecil |
| [`ui-ux-analyzer.md`](ui-ux-analyzer.md) | Instruksi kontekstual agar agent mengaudit dan mengikuti design system sebelum menulis kode UI | tidak ada | Sedang |
| [`create-engineering-task.md`](create-engineering-task.md) | Membuat engineering task modul dengan skill `engineering-task-gen` | `[nama_modul]` | Kecil |
| [`run-plan.md`](run-plan.md) | Menjalankan implementation plan per fase dengan konfirmasi antar fase | tidak ada | Kecil |
| [`THE COMPLETE HUMAN WRITING PROMPT.docx.md`](THE%20COMPLETE%20HUMAN%20WRITING%20PROMPT.docx.md) | Menulis ulang dokumen agar terbaca seperti tulisan manusia (bahasa Inggris) | tidak ada | Sangat besar (~76 KB) |

## Cara Pakai

1. Buka file prompt yang sesuai.
2. Ganti semua placeholder `[...]` dengan nilai nyata.
3. Tempel ke agent. Agar hasil bagus, jalankan di root proyek yang dianalisis.

Contoh `modul-analyzer.md` setelah diisi:

```text
susun rencana untuk membuat dokumentasi teknis modul Pasien (/pasien) tulis ke dalam file
@docs/features/pasien/pasien.md hanya membuat dokumentasi tanpa eksekusi kode apapun, ...

Analisa:
...
2. proses bisnis pendaftaran pasien baru
...
6. periksa FE dan BE apakah sudah sesuai dengan skill: vercel-react-best-practices
```

Untuk `run-plan.md`, pastikan implementation plan sudah ada di sesi (atau sebutkan file-nya) sebelum prompt dikirim.

## Detail Prompt

### `codebase-analyzer`

Peran: Senior Software Architect untuk tim yang baru menerima sebuah codebase.

- **Aman**: hanya menulis di `docs/_audit/`. Tidak menginstal, build, menjalankan aplikasi, atau memakai jaringan. Isi repo diperlakukan sebagai data tidak tepercaya, dan nilai secret tidak pernah disalin.
- **Berbasis bukti**: setiap klaim disertai sitasi `path:line`, lalu diverifikasi dengan skrip yang memeriksa semua sitasi.
- **Output** (narasi dalam Bahasa Indonesia): `INDEX.md`, `handover.md`, `tech-stack.md`, `architecture.md`, `business-flow.md`, `data-model.md`, `api.md`, `integrations.md`, `frontend.md`, `backend.md`, `security.md`, `testing.md`, `observability.md`. File hanya dibuat jika ada temuan.
- **Dapat dilanjutkan**: progres disimpan di `_progress.md`. Jika `docs/_audit/` sudah ada tanpa file itu, agent berhenti dan bertanya.
- Ditutup dengan laporan sesi dan tingkat *confidence* (High / Medium / Low).

### `modul-analyzer`

Menyusun rencana dokumentasi teknis satu modul tanpa menjalankan kode. Agent menganalisis:

1. pengguna yang terlibat
2. proses bisnis
3. komponen yang dipakai
4. tabel database
5. modul lain yang terkait
6. kesesuaian FE dan BE dengan skill yang ditentukan

Hasil ditulis ke `docs/features/[nama_modul]/[nama_modul].md`.

### `ui-ux-analyzer`

Peran: Lead Frontend Developer dan UX Specialist. Agent diminta membaca konfigurasi (`package.json`, `tailwind.config.js`, folder `theme/`/`styles/`), mencari contoh serupa, lalu mengikuti pola yang ada: framework styling, komponen yang bisa dipakai ulang, spacing, tipografi, warna, dark/light mode, copywriting, dan aksesibilitas. Ditutup dengan laporan: Ringkasan Audit, Temuan Utama, Rekomendasi Perubahan.

### `create-engineering-task`

Meminta agent membuat engineering task dengan skill [`engineering-task-gen`](../development/engineering-task-gen/SKILL.md), dengan prinsip YAGNI, KISS, dan DRY, dan membuat branch baru dari `main` bila perlu. Task disusun berurutan menurut prioritas dan ditulis agar model target (Sonnet 5.5 Medium) bekerja hemat token tanpa halusinasi. Ganti nama model target di prompt jika Anda memakai model lain.

### `run-plan`

Menjalankan implementation plan fase demi fase, dimulai dari fase satu. Setelah satu fase selesai, agent bertanya sebelum lanjut ke fase berikutnya.

### `THE COMPLETE HUMAN WRITING PROMPT`

Kerangka dua belas level (kata, kalimat, paragraf, pembuka, struktur, nada, spesifisitas, register, counter-argument, suara, bentuk makro, dan pembacaan ulang adversarial) untuk menulis ulang dokumen. Dua cara pakai:

- **Rewrite langsung**: tempel seluruh prompt, tambahkan "Rewrite the following document using this prompt", lalu tempel dokumennya.
- **Revisi mandiri**: pakai sebagai checklist per level.

Prompt ini memangkas panjang dokumen sekitar 20–40% dan ditulis dalam bahasa Inggris. Karena ukurannya besar, ia memakai banyak token konteks.

## Hubungan dengan Folder Lain

- [`../development/`](../development/README.md) berisi skill alur PRD → User Story → UI Spec → Engineering Task. Prompt `create-engineering-task` memanggil salah satunya.
- `modul-analyzer` cocok dijalankan sebelum membuat PRD pada modul yang sudah ada, atau untuk menyusun bahan `*-ai-context.md`.
