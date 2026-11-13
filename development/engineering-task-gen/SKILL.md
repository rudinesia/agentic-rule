---
name: engineering-task-gen
description: Skill untuk menganalisa PRD/User Story dan membuat dokumen Engineering Task (Blueprint Pelaksanaan) yang highly-actionable dan ter-standardisasi. Output wajib direview PM sebelum eksekusi kode.
---

# 📋 Engineering Task Generation Skill

Gunakan skill ini untuk membuat file `[feature]-task.md` sebagai panduan eksekusi teknis step-by-step (Blueprint) bagi AI Agent atau Developer.

> ⚠️ **WORKFLOW RULE**: Setelah dokumen task selesai dibuat, agent WAJIB berhenti dan menunggu konfirmasi PM sebelum mengeksekusi satu baris kode pun.

---

## 📋 Pre-Flight (WAJIB sebelum menulis)

1. Baca `AGENTS.md` di root — pastikan tidak ada konflik arsitektur global
2. Baca `[feature]-ai-context.md` — pahami state terkini fitur, bug yang sudah difix, dan anti-patterns
3. Baca PRD dan User Story yang menjadi referensi task ini
4. Jalankan File Discovery Protocol untuk semua file yang akan disentuh:
   ```bash
   # Temukan file yang berhubungan dengan fitur
   find src/ -iname "*[feature]*" -type f

   # Trace import dari entry point utama
   grep -r "[FeatureName]" src/ --include="*.ts" --include="*.tsx" -l
   ```
5. Jika ada ambiguitas teknis atau dependency yang belum jelas, **tanya dulu** — jangan asumsi

---

## 🏗️ Template Dokumen Task

```markdown
---
id: "[feature]-task"
feature: "[Nama Fitur]"
last_updated: "YYYY-MM-DD HH:mm"
prd_ref: "docs/features/[modul]/[modul]-prd.md"
us_ref: "docs/features/[modul]/[modul]-user-story.md"
status: "Draft / Ready / In Progress / Done"
---

# Engineering Task: [Nama Fitur] — Phase [X]

> **PURPOSE**: Blueprint eksekusi. Baca AGENTS.md dan ai-context sebelum mulai.

**Skills Wajib**: [Isi hanya skill yang benar-benar dibutuhkan fitur ini]
- `[skill-name]` — [alasan dibutuhkan]

> [!IMPORTANT]
> [Summary target phase ini & dependency krusial yang harus ada sebelum mulai]
> Contoh: "Tabel `master_penyakit` harus sudah exist sebelum task 1.1 dijalankan"

---

## 🔍 Konteks & Batasan (Agent Rules)

**DO:**
- Baca skill yang disebutkan di Skills Wajib sebelum eksekusi
- Terapkan Tenant Isolation RLS pada semua tabel baru
- Gunakan TanStack Query v5 untuk semua data fetching
- Tandai file path sebagai [VERIFIED] setelah dicek exist

**DON'T:**
- Modifikasi tabel atau komponen di luar scope task ini
- Hardcode user ID — selalu gunakan auth context
- Lanjut ke task berikutnya sebelum AC task saat ini tercentang semua
- [Tambahkan DON'T spesifik untuk fitur ini]

---

## 🛠️ Fase & Breakdown Task

> Urutan wajib: **Backend (DB) → Hooks (Data) → UI (Comp) → Page (Integrasi)**

---

### Task [1.1]: [Aksi singkat] — [Tabel/Komponen target]

**Skill**: `[skill-name]` | **US Ref**: `US-[MODUL]-[NO]` | **Estimasi**: [S/M/L]

**File yang Disentuh:**
| File | Action | Status |
|------|--------|--------|
| `src/[path]/[file].ts` | NEW / MODIFY | [VERIFIED] |

**❌ Out of Scope task ini:**
- [Hal spesifik yang tidak dikerjakan di task ini meskipun kelihatannya relevan]

**Spek Teknis:**
```sql
-- Draft SQL / RLS Policy
CREATE TABLE [nama_tabel] (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id uuid NOT NULL REFERENCES tenants(id),
  -- kolom lain
  created_at timestamptz DEFAULT now()
);

-- RLS
ALTER TABLE [nama_tabel] ENABLE ROW LEVEL SECURITY;
CREATE POLICY "tenant_isolation" ON [nama_tabel]
  USING (tenant_id = auth.jwt() ->> 'tenant_id');
```

```typescript
// Draft TS Interface
interface [NamaInterface] {
  id: string;
  // properti lain
}
```

**Acceptance Criteria:**
- [ ] [Kriteria 1 — atomik dan testable]
- [ ] [Kriteria 2]
- [ ] RLS policy aktif dan diverifikasi
- [ ] `npm run build` sukses — zero error, zero warning

**Verifikasi:**
```sql
-- Jalankan setelah task selesai untuk konfirmasi
SELECT * FROM [nama_tabel] LIMIT 5;
-- Expected: [hasil yang diharapkan]
```

---

### Task [1.2]: [Aksi singkat] — [Tabel/Komponen target]

[Ulangi struktur yang sama]

---

### Task [2.1]: [Aksi singkat] — [Hook/Data layer]

[Ulangi struktur yang sama]

---

## 🧪 Fase Final: Verifikasi & QA

### Per-Task Checklist (agent isi sebelum lapor selesai)
- [ ] Semua AC di setiap task sudah tercentang
- [ ] Tidak ada task yang di-skip atau di-partial
- [ ] Semua file `[INFERRED]` sudah dikonfirmasi atau dilaporkan ke PM

### Data Integrity
```sql
-- Verifikasi RLS aktif di semua tabel baru
SELECT tablename, rowsecurity
FROM pg_tables
WHERE schemaname = 'public'
AND tablename IN ('[tabel1]', '[tabel2]');
-- Expected: rowsecurity = true untuk semua tabel
```

### Build & Type Check
- [ ] `npm run build` — zero error
- [ ] `npm run typecheck` — zero error
- [ ] Tidak ada `console.error` di browser

### Docs Update (wajib setelah semua task selesai)
- [ ] Update `CHANGELOG.md`
- [ ] Update `ARRUMI-SCHEMA-PROD.sql` jika ada perubahan DB
- [ ] Update `APP_ARCHITECTURE.md` jika ada perubahan struktur
- [ ] Update `[feature]-ai-context.md` — Section 7 Session Handoff

---

## 💡 Best Practices

- **Granularity**: 1 Task = 1 langkah logis yang bisa di-test mandiri
- **Code First**: Draft SQL dan TS interface wajib ada di dalam task — mencegah halusinasi saat eksekusi
- **Traceability**: Setiap task terhubung ke US-ID
- **Verified Files Only**: Jangan tulis path yang belum dicek exist
- **Out of Scope per task**: Lebih baik eksplisit daripada agent over-engineer

---

## 📋 Checklist Sebelum Output ke PM

**Agent mengisi ini sebelum menyerahkan dokumen:**
- [ ] Pre-Flight sudah dijalankan (AGENTS.md + ai-context sudah dibaca)
- [ ] File Discovery Protocol sudah dijalankan — semua path di-tag [VERIFIED] atau [INFERRED]
- [ ] Skills Wajib hanya berisi skill yang benar-benar dibutuhkan (tidak hardcode)
- [ ] Setiap task punya Out of Scope yang eksplisit
- [ ] Draft SQL/TS interface ada di setiap task yang relevan
- [ ] Fase Final QA sudah diisi lengkap
- [ ] Tidak ada asumsi yang belum dikonfirmasi PM

---

## 🛑 Mandatory Stop — Menunggu Review PM

Setelah dokumen task selesai, agent WAJIB menulis pesan berikut dan BERHENTI:

```
✅ Engineering Task sudah selesai dibuat.

📄 File yang dibuat:
- docs/features/[modul]/[modul]-task.md

📋 Summary task yang akan dikerjakan:
- Task 1.1: [deskripsi singkat]
- Task 1.2: [deskripsi singkat]
- Task 2.1: [deskripsi singkat]
- [dst]

⚠️ File [INFERRED] yang perlu dikonfirmasi sebelum eksekusi:
- [path] — [alasan belum bisa diverifikasi] (atau tulis "Tidak ada" jika semua verified)

🔍 Mohon review dokumen di atas sebelum eksekusi.
Jika sudah sesuai, ketik: "Mulai eksekusi Task [X.X]"
Jika ada revisi, sebutkan task mana yang perlu diubah.

⏸️ Saya menunggu konfirmasi sebelum menulis kode apapun.
```
