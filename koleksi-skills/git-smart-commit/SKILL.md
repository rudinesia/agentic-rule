---
name: git-smart-commit
description: Automasi Git commit dengan analisis AI, format Conventional Commits, dan multiple -m flags untuk keterbacaan log.
---

# Git Smart Commit Skill

Gunakan skill ini untuk melakukan `commit` pada *staged changes* secara cerdas.

## 🛠️ Alur Kerja (Workflow)

1.  **Status Check**: Jalankan `git diff --cached --name-only`.
    - Jika kosong: Berhenti. Minta user melakukan `git add`.
2.  **Analysis**: Jalankan `git diff --cached` untuk memahami perubahan.
3.  **Generate Message**: Buat pesan commit (Bahasa Inggris) sesuai aturan di bawah.
4.  **Execution**: Jalankan `git commit` menggunakan format **Multiple -m Flags**.

## 📝 Aturan Pesan Commit (Conventional Commits v1.0.0)

### Format Perintah
`git commit -m "<type>(<scope>): <subject>" -m "- <detail 1>" -m "- <detail 2>"`

### Komponen Pesan
- **Type**: Pilih dari daftar di bawah (wajib).
- **Scope**: Nama modul/file yang berubah, misal: `(auth)`, `(ui)`, `(api)` (opsional).
- **Subject**: Deskripsi singkat, imperatif, lowercase, tanpa titik di akhir (wajib).
- **Body (-m selanjutnya)**: Detail teknis per poin, diawali `- `. **Wajib `-m` terpisah per poin**.

### Daftar Tipe (Types)
| Tipe | Deskripsi | Tipe | Deskripsi |
| :--- | :--- | :--- | :--- |
| `feat` | Fitur baru | `fix` | Perbaikan bug |
| `refactor` | Perizinan kode (bukan feat/fix) | `perf` | Optimasi performa |
| `docs` | Dokumentasi saja | `style` | Formatting/White-space |
| `build` | Sistem build/deps (`vite`, `npm`) | `ci` | Konfigurasi CI/CD |
| `test` | Tambah/perbaiki tests | `chore` | Lain-lain (bukan src/test) |

### Ketentuan Teknis
- **Imperative**: Gunakan "add" bukan "added" atau "adds".
- **Breaking Change**: Tambah `!` setelah type/scope (contoh: `feat(api)!: change endpoint`).
- **No Newlines**: Jangan gunakan `\n`. Gunakan flag `-m` berulang untuk baris baru.
- **Language**: Isi pesan (Subject & Body) WAJIB Bahasa Inggris.

## 💡 Contoh Eksekusi
**Input**: Perubahan regex email di `Login.tsx` dan penambahan test.
**Command**:
```bash
git commit -m "fix(auth): correct email regex and add validation tests" -m "- update regex to support specialized domains" -m "- add unit tests for edge case emails"
```
