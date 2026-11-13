# agentic-rule

Kumpulan rule, skill, dan prompt untuk agentic AI (vibe coding): dari kebutuhan produk sampai commit, plus prompt analisis dan skill landing page.

## Isi Repositori

| Folder | Isi | Detail |
|--------|-----|--------|
| [`development/`](development/) | Skill alur pengembangan: PRD → User Story → UI/UX Spec → Engineering Task, AI Context untuk merangkum fase dan kondisi modul, dan Git Smart Commit. | [README](development/README.md) |
| [`kumpulan-prompt/`](kumpulan-prompt/) | Prompt siap pakai: audit codebase, dokumentasi modul, analisis UI/UX, pembuatan engineering task, menjalankan plan per fase, dan penulisan ulang dokumen. | [README](kumpulan-prompt/README.md) |
| [`landing-page/`](landing-page/) | Skill landing page: strategi, pola konversi, dan standar teknis. | [README](landing-page/README.md) |

## Mulai Cepat

- Membangun fitur baru dengan AI agent? Mulai dari [`development/`](development/README.md) dan ikuti urutan skill-nya.
- Perlu memahami codebase atau modul yang sudah ada? Pakai `codebase-analyzer` atau `modul-analyzer` di [`kumpulan-prompt/`](kumpulan-prompt/README.md).
- Membuat landing page? Mulai dari `lp-strategy` di [`landing-page/`](landing-page/README.md).

Cara memasang skill: salin folder skill yang dibutuhkan ke direktori skill agent Anda (misalnya `.claude/skills/`).

## Lisensi

Lihat [LICENSE](LICENSE).
