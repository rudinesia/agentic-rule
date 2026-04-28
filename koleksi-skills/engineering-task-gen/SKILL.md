---
name: engineering-task-gen
description: Skill untuk menganalisa PRD/User Story dan membuat dokumen Engineering Task (Blueprint Pelaksanaan) yang highly-actionable dan ter-standardisasi.
---

# 📋 Engineering Task Generation Skill

Gunakan skill ini untuk membuat file `[feature]-task.md` sebagai panduan eksekusi teknis step-by-step (Blueprint) bagi AI Agent atau Developer.

## 🏗️ Struktur Wajib (Template)

```markdown
# Engineering Task: [Nama Fitur] — [Fase]

**Refs**: [PRD-Link] | [UserStory-Link] | [Last-Updated]
**Skills Wajib**: `supabase-migration`, `react-query-hook`, `shadcn-component`

> [!IMPORTANT]
> [Summary target & Dependencies krusial (misal: Tabel Master harus ada)]

## 🔍 Konteks & Batasan (Agent Rules)
- **DO**: Baca `SKILL.md` sebelum eksekusi, Tenant Isolation RLS (Wajib), TanStack Query v5.
- **DON'T**: Modifikasi tabel/komponen di luar scope, Hardcode ID (Gunakan auth context).

## 🛠️ Fase & Breakdown Task (Deterministic & Testable)

### Task [Fase.No]: [Aksi] — [Komponen/Tabel]
**Skill**: `[skill-name]` | **US Ref**: `[US-ID]` | **File**: `[path]` ([NEW/MODIFY])

**Spek Teknis**:
- [Logic/Business Rules]
- [Draft SQL Schema / TS Interfaces / Wireframe Layout]

**Acceptance Criteria (AC)**:
- [ ] [Kriteria atomik & testable]
- [ ] `npm run build` sukses (Zero error)

**Verifikasi**: [Snippet SQL atau Skenario UI untuk validasi keberhasilan]

## 🧪 Fase Final: Verifikasi & QA
- **Data Integrity**: SQL check RLS & constraints.
- **Build**: Zero TS errors/warnings.
- **Docs**: Update `CHANGELOG.md`, `ARRUMI-SCHEMA-PROD.sql`, dan `APP_ARCHITECTURE.md`.
```

## 💡 Best Practices (Context7 Standard)
- **Granularity**: 1 Task = 1 Langkah logis yang bisa di-test mandiri.
- **Code First**: Sertakan query SQL dasar, interface TS, dan struktur komponen di dalam task (cegah halusinasi).
- **Traceability**: Selalu hubungkan sub-task ke ID User Story (`US-ID`).
- **Phasing**: Urutkan dari **Backend (DB) → Hooks (Data) → UI (Comp) → Page (Integrasi)**.

## 📋 Checklist Review Dokumen Task
- [ ] Header Metadata lengkap.
- [ ] DOs & DON'Ts spesifik untuk fitur tersebut.
- [ ] SQL Schema dan RLS Policy sudah di-draft.
- [ ] Verifikasi QA (Final Phase) disertakan.
- [ ] Path file sudah diverifikasi (Absolute/Relative correct).
