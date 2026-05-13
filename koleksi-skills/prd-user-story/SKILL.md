---
name: prd-user-story
description: Skill untuk membuat PRD dan User Story untuk fitur baru. Output wajib direview PM sebelum lanjut ke Engineering Task.
---

# PRD & User Story Skill

Gunakan skill ini untuk membuat dokumentasi kebutuhan produk (PRD) dan alur pengguna (User Story) terstandarisasi.

> ⚠️ **WORKFLOW RULE**: Setelah output selesai, agent WAJIB berhenti dan menunggu konfirmasi PM sebelum melanjutkan ke Engineering Task atau eksekusi apapun.

## 📋 Pre-Flight (WAJIB sebelum menulis)
1. Baca `AGENTS.md` di root — pastikan tidak ada konflik arsitektur
2. Konfirmasi kode modul dengan tabel di bawah
3. Jika ada ambiguitas requirement, **tanya dulu** — jangan asumsi

---

## 📂 Lokasi File & Penamaan
- **PRD**: `docs/features/[modul]/[modul]-prd.md`
- **User Story**: `docs/features/[modul]/[modul]-user-story.md`
- **Link**: WAJIB Relative Path. DILARANG absolute path (`C:/`, `file:///`).

---

## 🏗️ Template PRD

```markdown
---
id: "[modul]-prd"
feature: "[Nama Fitur]"
last_updated: "YYYY-MM-DD HH:mm"
status: "Draft / Review / Approved"
---

# PRD: [Nama Fitur]

## 1. 🎯 Ringkasan
> Mengapa fitur ini dibangun? (maks 3 kalimat)

[Isi di sini]

## 2. 🏆 Goals
| Tipe | Goal |
|------|------|
| Business | [Misal: Meningkatkan retensi user 20%] |
| User | [Misal: User bisa lihat rekap tanpa scroll panjang] |

## 3. ✅ Functional Requirements
### Must-Have
| ID | Requirement | Acceptance Criteria |
|----|-------------|---------------------|
| FR-01 | [Deskripsi singkat] | [Kondisi yang harus terpenuhi] |
| FR-02 | [Deskripsi singkat] | [Kondisi yang harus terpenuhi] |

### Should-Have (nice to have, bukan blocker)
| ID | Requirement | Acceptance Criteria |
|----|-------------|---------------------|
| FR-S01 | [Deskripsi singkat] | [Kondisi yang harus terpenuhi] |

### ❌ Out of Scope
- [Fitur X] — tidak dikerjakan di fase ini
- [Integrasi Y] — akan dipertimbangkan di roadmap berikutnya

## 4. ⚙️ Non-Functional Requirements
| Aspek | Requirement |
|-------|-------------|
| Performance | Page load < 2s, API response < 500ms |
| RLS | [Tenant Isolation / Global / Custom] |
| Mobile | [Responsive / Drawer / Dialog — spesifikkan] |
| Accessibility | [jika relevan] |

## 5. 🗄️ Data Model
### Tabel Baru / Diubah
```sql
-- [nama_tabel]
CREATE TABLE [nama_tabel] (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  -- kolom lain
);
```

### ERD (Mermaid)
```mermaid
erDiagram
  [TABEL_A] ||--o{ [TABEL_B] : "relasi"
```

## 6. 🔄 Logic & Flow
### Happy Path
1. User melakukan [X]
2. System merespons dengan [Y]
3. User melihat [Z]

### Edge Cases
| Kondisi | Perilaku yang Diharapkan |
|---------|--------------------------|
| [Misal: Data kosong] | [Tampilkan empty state] |
| [Misal: Offline] | [Tampilkan pesan error + retry] |
| [Misal: Conflict data] | [Strategi resolusi] |

## 7. 📊 Success Metrics
| Metrik | Target |
|--------|--------|
| [Misal: Load time] | [< 2s] |
| [Misal: Error rate] | [< 1%] |
```

---

## 🏗️ Template User Story

```markdown
---
id: "[modul]-user-story"
feature: "[Nama Fitur]"
last_updated: "YYYY-MM-DD HH:mm"
prd_ref: "[modul]-prd.md"
---

# User Story: [Nama Fitur]

## US-[MODUL]-01: [Verb Noun singkat, misal: View Dashboard Stats]

**Story Statement**
> Sebagai [Role], saya ingin [Fitur], agar [Manfaat].

**❌ Out of Scope**
- [Hal spesifik yang TIDAK dikerjakan di story ini]

**Acceptance Criteria (Gherkin)**
```gherkin
Scenario 1: Happy Path
  Given [kondisi awal]
  When  [aksi user]
  Then  [hasil yang diharapkan]

Scenario 2: Validation Error
  Given [kondisi awal]
  When  [aksi user dengan input salah]
  Then  [pesan error yang muncul]

Scenario 3: Edge Case (jika ada)
  Given [kondisi edge]
  When  [aksi]
  Then  [perilaku sistem]
```

**Implementation Notes**
| Aspek | Detail |
|-------|--------|
| Table | `[nama_tabel]` |
| RLS Policy | [Tenant Isolation / Global] |
| Route | `[/path/to/page]` |
| Components | `[NamaKomponen.tsx]` (jika sudah ada) |
| State | React Query / Zustand / Server State |

**Definition of Done**
- [ ] Semua AC terpenuhi
- [ ] Linter pass (no error, no warning)
- [ ] RLS policy diverifikasi
- [ ] Mobile behavior sesuai spesifikasi NFR
- [ ] Tidak ada console.error di browser
```

---

## 🏷️ Kode Modul Standard
| Kode | Modul |
|------|-------|
| `AUTH` | Authentication |
| `DASH` | Dashboard |
| `PROF` | Profil |
| `ADM` | Admin |

> Jika modul belum ada di tabel, **tanya PM** sebelum membuat kode baru.

---

## ✅ Checklist Sebelum Output ke PM

**Agent mengisi ini sebelum menyerahkan dokumen:**
- [ ] Pre-Flight sudah dijalankan (AGENTS.md sudah dibaca)
- [ ] Semua requirement atomic & testable
- [ ] Out of Scope sudah didefinisikan di PRD dan tiap User Story
- [ ] Edge cases (Offline, Conflict, Empty State) terdefinisi
- [ ] RLS Policy sudah spesifik (Tenant vs Global)
- [ ] Mobile behavior didefinisikan (Drawer / Dialog / Responsive)
- [ ] Tidak ada absolute file path
- [ ] Tidak ada asumsi yang belum dikonfirmasi PM

---

## 🛑 Mandatory Stop — Menunggu Review PM

Setelah dokumen selesai, agent WAJIB menulis pesan berikut dan BERHENTI:

```
✅ PRD dan User Story sudah selesai dibuat.

📄 File yang dibuat:
- docs/features/[modul]/[modul]-prd.md
- docs/features/[modul]/[modul]-user-story.md

🔍 Mohon review dokumen di atas.
Jika sudah sesuai, ketik: "Lanjut Engineering Task"
Jika ada revisi, sebutkan bagian yang perlu diubah.

⏸️ Saya menunggu konfirmasi sebelum melanjutkan.
```
