# Development Skills

Kumpulan skill untuk *vibe coding* dengan AI agent. Skill-skill ini memandu agent dari ide fitur sampai kode ter-commit, dengan dokumen terstandar dan checkpoint review PM di setiap tahap.

## Daftar Isi

- [Alur Kerja](#alur-kerja)
- [Daftar Skill](#daftar-skill)
- [Cara Pakai](#cara-pakai)
- [Struktur Folder](#struktur-folder)
- [Aturan Bersama](#aturan-bersama)

## Alur Kerja

```
PRD → User Story → UI/UX Spec → Engineering Task → Eksekusi → Git Commit
 │         │            │              │               │
 └─────────┴────────────┴──────────────┴───────────────┘
              AI Context (rangkuman fase & kondisi modul)
```

Setiap dokumen harus **disetujui PM** sebelum lanjut ke tahap berikutnya. Agent berhenti dan menunggu konfirmasi di akhir tiap skill (*Mandatory Stop*).

## Daftar Skill

### Pipeline dokumen (urut)

| # | Skill | Output | Fungsi |
|---|-------|--------|--------|
| 1 | [`prd-user-story`](prd-user-story/SKILL.md) | `[modul]-prd.md`, `[modul]-user-story.md` | Kebutuhan produk: goals, functional/non-functional requirement, data model, edge case, dan user story dengan acceptance criteria Gherkin. |
| 2 | [`ui-ux-spec`](ui-ux-spec/SKILL.md) | `[modul]-ui-spec.md` | Blueprint tampilan berbasis teks: user flow, wireframe ASCII (desktop & mobile), component breakdown (EXIST/NEW), interaction states. |
| 3 | [`engineering-task-gen`](engineering-task-gen/SKILL.md) | `[modul]-task.md` | Blueprint eksekusi step-by-step: urutan DB → hooks → UI → page, draft SQL/TypeScript, acceptance criteria, dan QA akhir. |

### Pendukung

| Skill | Output | Fungsi |
|-------|--------|--------|
| [`ai-context-doc`](ai-context-doc/SKILL.md) | `[feature]-ai-context.md` | Merangkum PRD, User Story, dan Task menjadi satu *master reference* (< 10 KB): arsitektur, DB & RLS, anti-pattern, file terkait (verified), bug yang sudah difix, roadmap, dan **Session Handoff** antar sesi. |
| [`git-smart-commit`](git-smart-commit/SKILL.md) | Commit | Membuat commit dari *staged changes* dengan format Conventional Commits dan beberapa flag `-m` (pesan dalam Bahasa Inggris). |

## Cara Pakai

Salin folder skill yang dibutuhkan ke direktori skill agent Anda (misalnya `.claude/skills/`), lalu minta agent menjalankannya secara berurutan.

**1. PRD & User Story**

Minta agent membuat PRD dan User Story untuk fitur baru. Jika kode modul belum terdaftar, agent akan bertanya dulu. Setelah review, balas:

```
Lanjut UI Spec
```

**2. UI/UX Spec**

Dijalankan setelah PRD dan User Story disetujui. Kata kunci pemicu: "buat UI spec", "wireframe", "user flow", "tampilan mobile".

**3. Engineering Task**

Agent membaca `AGENTS.md` dan `[feature]-ai-context.md`, memverifikasi file yang akan disentuh, lalu menyusun task. Eksekusi kode dimulai hanya setelah Anda menjawab:

```
Mulai eksekusi Task 1.1
```

**4. AI Context**

Perbarui di akhir setiap sesi (bagian *Session Handoff*) agar sesi berikutnya langsung tahu posisi terakhir. Di sesi baru, minta agent membaca file ini lebih dulu.

**5. Git Smart Commit**

Stage perubahan, lalu panggil skill:

```bash
git add <file>
# minta agent: "commit perubahan"
```

Contoh hasil:

```bash
git commit -m "fix(auth): correct email regex and add validation tests" -m "- update regex to support specialized domains" -m "- add unit tests for edge case emails"
```

## Struktur Folder

```
development/
├── prd-user-story/        # Tahap 1: PRD + User Story
├── ui-ux-spec/            # Tahap 2: UI/UX Spec
├── engineering-task-gen/  # Tahap 3: Engineering Task
├── ai-context-doc/        # Rangkuman fase & kondisi modul
└── git-smart-commit/      # Bantuan commit
```

Dokumen yang dihasilkan disimpan di proyek target pada `docs/features/[modul]/`.

## Aturan Bersama

- **Pre-flight**: agent wajib membaca `AGENTS.md` di root proyek sebelum menulis dokumen.
- **Tanya, jangan asumsi**: jika ada ambiguitas, agent bertanya ke PM terlebih dahulu.
- **Mandatory Stop**: tiap dokumen diakhiri permintaan review; agent tidak lanjut tanpa konfirmasi.
- **Verified-only**: path file hanya ditulis setelah dicek ada di codebase (`[VERIFIED]`), selain itu ditandai `[INFERRED]` dan dikonfirmasi.
- **Relative path**: tautan antar dokumen memakai path relatif, bukan absolut.
