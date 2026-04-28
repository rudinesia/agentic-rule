---
name: ai-context-doc
description: Skill untuk membuat dan memperbarui dokumen AI Context (`*-ai-context.md`) yang merangkum PRD, User Story, dan Engineering Task menjadi satu Master Reference.
---
 
# 🤖 AI Context Document Skill
 
Gunakan skill ini untuk membuat/memperbarui file `[feature]-ai-context.md` sebagai **Master Ground-Truth** bagi AI Agent antar sesi.
 
## 📐 Prinsip Desain (Target Size: < 10 KB)
- **Token-Efficient**: Gunakan tabel, bullet points, dan `inline-code`. Hindari narasi.
- **Actionable**: Fokus pada "apa yang harus dilakukan" dan "apa yang sudah ada".
- **Single Source of Truth**: Rangkum PRD, User Story, dan Task. Arahkan agent baru hanya membaca file ini.
- **Verified-Only**: Jangan tulis asumsi — hanya fakta yang sudah diverifikasi dari codebase.
## 🔍 File Discovery Protocol (WAJIB sebelum mengisi Section 4)
 
Agent **DILARANG** menulis file path dari ingatan/asumsi. Lakukan langkah berikut terlebih dahulu:
 
```bash
# 1. Temukan file yang berhubungan dengan fitur
find src/ -iname "*[feature]*" -type f
 
# 2. Trace import dari entry point utama
grep -r "[FeatureName]" src/ --include="*.ts" --include="*.tsx" -l
 
# 3. Cek file config/route yang mungkin terdampak
grep -r "[route/endpoint]" src/ --include="*.ts" -l
```
 
**Aturan penandaan file:**
| Status | Arti |
|--------|------|
| `[VERIFIED]` | File sudah dicek exist di codebase |
| `[INFERRED]` | Belum dicek — tandai, laporkan ke user untuk konfirmasi |
 
> ⚠️ Jika sebuah file tidak bisa diverifikasi, **jangan tulis** — lebih baik kosong daripada salah.
 
---
 
## 🏗️ Struktur Wajib (Template)
 
```markdown
---
id: "[feature]-ai-context"
subject: "[Nama Fitur] Module ARRUMI"
last_updated: "YYYY-MM-DD HH:MM:SS"
status: "Phase X Completed / In Progress"
---
 
# 🤖 AI Context: [Nama Modul]
 
> **PURPOSE**: Master reference. Do not refer to legacy docs; strictly use this.
> **AGENTS.md**: Baca `AGENTS.md` di root untuk keputusan arsitektur global dan skill yang tersedia sebelum mulai.
 
## 1. 🏗️ Architecture & Paradigm
- **Pola**: [Misal: Multistep Form / Realtime Chart]
- **Ketergantungan**: [Misal: Master Penyakit / Profil Puskesmas]
 
## 2. 🗄️ Database & RLS
| Table | Fungsi | RLS Policy |
|-------|--------|------------|
| `table` | Deskripsi | [Tenant Isolation / Global] |
 
## 3. 🧠 Business Logic & Anti-Patterns
- **Logic**: `[Formula/Rule]`
- **Gotcha**: [Hal yang sering salah]
- **[ANTI-PATTERN]**: JANGAN lakukan [X], gunakan [Y].
 
## 4. 📁 Related Files (Verified Only)
| File | Role | Status |
|------|------|--------|
| `src/hooks/use[Name].ts` | [State management / API call / dll] | [VERIFIED] |
| `src/components/[Name].tsx` | [UI Layer / Form / dll] | [VERIFIED] |
 
> File bertanda [INFERRED] harus dikonfirmasi sebelum dimodifikasi.
 
## 5. 🐛 Known Bugs & Fixes
| Symptom | Root Cause | Resolution |
|---------|------------|------------|
| [Issue] | [Cause] | [Fix applied] |
 
## 6. 🚀 Next Roadmap
1. **[Task Name]**: Deskripsi singkat.
 
## 7. 🔄 Session Handoff
> Diisi agent di AKHIR setiap sesi. Wajib diperbarui sebelum user pindah ke sesi baru.
 
- **Terakhir dikerjakan**: [deskripsi singkat apa yang baru selesai]
- **Berhenti di**: [file / fungsi / baris terakhir yang disentuh]
- **Status task**: [Done ✅ / Partial 🔄 / Blocked ❌]
- **Hati-hati di sesi berikutnya**: [warning teknis spesifik jika ada]
- **Jangan lupa**: [hal yang belum selesai atau perlu di-follow up]
```
 
---
 
## 📋 Alur Kerja (Workflow)
 
1. **Gather**: Baca parallel PRD, User Story, dan Task file.
2. **Discover**: Jalankan File Discovery Protocol — verifikasi semua file path sebelum ditulis.
3. **Distill**: Ekstrak skema DB, logic kritis, dan bug yang sudah difix.
4. **Verify**: Pastikan semua path di Section 4 bertanda `[VERIFIED]`.
5. **Handoff**: Isi Section 7 di akhir sesi sebelum user menutup konteks.
6. **Commit**: `docs([feature]): update ai-context for phase [X]`
---
 
## ⚡ Session Start Checklist (untuk agent di sesi baru)
 
Ketika membuka sesi baru dengan context file ini, agent WAJIB:
 
- [ ] Baca `AGENTS.md` di root terlebih dahulu
- [ ] Baca Section 7 (Session Handoff) dari sesi sebelumnya
- [ ] Konfirmasi file `[INFERRED]` di Section 4 sebelum menyentuhnya
- [ ] Klarifikasi scope: *"Saya akan mengerjakan X, menyentuh file Y dan Z. Apakah ada yang perlu dikecualikan?"*