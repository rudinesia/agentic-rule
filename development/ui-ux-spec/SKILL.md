---
name: ui-ux-spec
description: Skill untuk membuat dokumen UI/UX Specification (`[feature]-ui-spec.md`) sebagai design blueprint berbasis teks — mencakup wireframe ASCII, user flow, component breakdown, interaction states, dan mobile behavior. Gunakan skill ini SETELAH PRD dan User Story disetujui PM, dan SEBELUM Engineering Task dibuat. Trigger skill ini ketika user menyebut "buat UI spec", "rancangan tampilan", "wireframe", "user flow", "desain halaman", "komponen apa saja", "tampilan mobile", atau meminta review visual sebelum eksekusi kode.
---

# 🎨 UI/UX Specification Skill

Gunakan skill ini untuk membuat file `[feature]-ui-spec.md` sebagai **Design Blueprint** yang bisa direview PM sebelum Engineering Task dibuat dan kode dieksekusi.

> ⚠️ **WORKFLOW RULE**: Setelah dokumen selesai, agent WAJIB berhenti dan menunggu konfirmasi PM. Tidak ada eksekusi kode atau pembuatan Engineering Task sebelum PM menyetujui dokumen ini.

---

## 📍 Posisi dalam Workflow

```
PRD + User Story (Approved)
        ↓
  [UI/UX Spec]  ← Skill ini
        ↓
 Engineering Task
        ↓
    Eksekusi
```

---

## 📋 Pre-Flight (WAJIB sebelum menulis)

1. Baca `AGENTS.md` di root — cek component library, design system, dan UI conventions
2. Baca PRD yang sudah diapprove — fokus pada NFR (Mobile behavior, Accessibility)
3. Baca User Story yang sudah diapprove — pahami flow dan AC tiap story
4. Baca `[feature]-ai-context.md` jika ada — cek komponen yang sudah exist
5. Jika ada ambiguitas layout atau interaksi, **tanya PM dulu** — jangan asumsi

---

## 📂 Lokasi File & Penamaan

- **UI Spec**: `docs/features/[modul]/[modul]-ui-spec.md`
- **Link**: WAJIB Relative Path

---

## 🏗️ Template Dokumen

```markdown
---
id: "[modul]-ui-spec"
feature: "[Nama Fitur]"
last_updated: "YYYY-MM-DD"
prd_ref: "docs/features/[modul]/[modul]-prd.md"
us_ref: "docs/features/[modul]/[modul]-user-story.md"
status: "Draft / Review / Approved"
---

# 🎨 UI/UX Spec: [Nama Fitur]

> **PURPOSE**: Design blueprint untuk review PM sebelum Engineering Task dibuat.
> Tidak ada kode yang ditulis sebelum dokumen ini diapprove.

---

## 1. 🗺️ User Flow Overview

> Gambaran besar perjalanan user dari entry point sampai goal tercapai.

```
[Entry Point] → [Halaman/State A] → [Aksi User] → [Halaman/State B] → [Goal]

Contoh:
[Menu Sidebar] → [List Pasien] → [Klik "Tambah"] → [Form Input] → [Submit] → [Toast Sukses + Redirect]
```

**Flow Alternatif (Error/Edge):**
```
[Form Submit] → [Validasi Gagal] → [Inline Error Message] → [User Perbaiki] → [Submit Ulang]
[Form Submit] → [Network Error] → [Toast Error + Tetap di Form]
```

---

## 2. 📐 Wireframe per Halaman/View

> Gunakan ASCII wireframe. Fokus pada layout dan hierarki konten, bukan pixel-perfect.

### View: [Nama Halaman — misal: List Data]

**Desktop Layout:**
```
┌─────────────────────────────────────────────────┐
│ [Page Title]                    [+ Tambah Button]│
├─────────────────────────────────────────────────┤
│ [Search Bar]              [Filter Dropdown ▼]    │
├────────┬───────────┬───────────┬────────────────┤
│ Kolom  │ Kolom     │ Status    │ Aksi           │
├────────┼───────────┼───────────┼────────────────┤
│ [Data] │ [Data]    │ [Badge]   │ [Edit][Delete] │
│ [Data] │ [Data]    │ [Badge]   │ [Edit][Delete] │
├────────┴───────────┴───────────┴────────────────┤
│              [Prev | 1 2 3 | Next]              │
└─────────────────────────────────────────────────┘
```

**Mobile Layout:**
```
┌─────────────────────┐
│ [Page Title]    [+] │
├─────────────────────┤
│ [Search Bar]        │
│ [Filter ▼]          │
├─────────────────────┤
│ ┌─────────────────┐ │
│ │ [Judul Item]    │ │
│ │ [Sub info]      │ │
│ │ [Badge Status]  │ │
│ │ [Edit] [Delete] │ │
│ └─────────────────┘ │
│ ┌─────────────────┐ │
│ │ [Card 2...]     │ │
│ └─────────────────┘ │
├─────────────────────┤
│   [Load More ↓]     │
└─────────────────────┘
```

**Catatan layout:**
- Mobile: card list, bukan tabel
- Tombol tambah di mobile: FAB kanan bawah
- [Catatan spesifik lainnya]

---

### View: [Nama Halaman — misal: Form Tambah/Edit]

**Desktop Layout:**
```
┌─────────────────────────────────────────────────┐
│ ← Kembali        [Judul Form]                   │
├─────────────────────────────────────────────────┤
│                                                 │
│  Field Satu *                                   │
│  [________________________________]             │
│                                                 │
│  Field Dua *              Field Tiga            │
│  [________________]       [Dropdown ▼_______]   │
│                                                 │
│  Field Panjang (textarea)                       │
│  [________________________________]             │
│  [________________________________]             │
│                                                 │
│  [Batal]                        [Simpan →]      │
└─────────────────────────────────────────────────┘
```

**Mobile Layout:**
```
┌─────────────────────┐
│ ← [Judul Form]      │
├─────────────────────┤
│ Field Satu *        │
│ [_______________]   │
│                     │
│ Field Dua *         │
│ [_______________]   │
│                     │
│ Field Tiga          │
│ [Dropdown ▼______]  │
│                     │
│ Field Panjang       │
│ [_______________]   │
│ [_______________]   │
├─────────────────────┤
│ [Batal]  [Simpan →] │
└─────────────────────┘
```

---

## 3. 🧩 Component Breakdown

> Semua komponen UI yang dibutuhkan. Tandai EXIST (sudah ada) vs NEW (perlu dibuat).

| Komponen | Tipe | Lokasi / Referensi | Status |
|----------|------|--------------------|--------|
| `PageHeader` | Layout | `src/components/ui/PageHeader.tsx` | EXIST |
| `DataTable` | Data Display | shadcn/ui `Table` | EXIST |
| `StatusBadge` | Display | `src/components/ui/StatusBadge.tsx` | EXIST |
| `SearchInput` | Form | shadcn/ui `Input` | EXIST |
| `[NamaKomponen]` | [Tipe] | [Referensi] | NEW |

---

## 4. 🔄 Interaction States

> Semua state yang harus ditangani — bukan hanya happy path.

### State per View

| State | Trigger | Tampilan |
|-------|---------|----------|
| Loading | Data sedang difetch | Skeleton loader |
| Empty | Tidak ada data | Ilustrasi + teks + tombol aksi |
| Error | API gagal | Toast error + tombol "Coba Lagi" |
| Success | Aksi berhasil | Toast sukses + refresh data |
| Confirming | Klik Delete | ConfirmDialog muncul |
| Submitting | Form di-submit | Tombol disabled + spinner |

### Form Validation States

| Field | Validasi | Pesan Error |
|-------|----------|-------------|
| [Field 1] | Required, min X char | "[Pesan error]" |
| [Field 2] | Format valid | "[Pesan error]" |

---

## 5. 📱 Mobile Behavior

| Elemen | Desktop | Mobile |
|--------|---------|--------|
| Layout data | Tabel | Card list |
| Tombol tambah | Button di header | FAB kanan bawah |
| Form fields | Side-by-side | Full-width stacked |
| Modal/Dialog | Dialog centered | Bottom sheet / Drawer |
| Pagination | Row angka | Load more button |

---

## 6. ❌ Out of Scope (UI)

- [Misal: Dark mode — akan dipertimbangkan di fase berikutnya]
- [Misal: Animasi transisi antar halaman]
- [Misal: Export PDF dari view ini]

---

## 7. 📝 Catatan untuk Engineering Task

> Hint teknis singkat untuk agent saat implementasi nanti.

- Komponen `[X]` sudah ada di `src/components/ui/` — reuse, jangan buat ulang
- State halaman ini cukup React Query — tidak perlu Zustand
- [Catatan teknis singkat lainnya]
```

---

## 💡 Panduan Menulis Wireframe ASCII

| Elemen | Karakter |
|--------|----------|
| Border box | `┌ ┐ └ ┘ │ ─` |
| Separator dalam | `├ ┤ ┬ ┴ ┼` |
| Input field | `[___________]` |
| Button | `[Label Button]` |
| Dropdown | `[Pilihan ▼]` |
| Data/placeholder | `[Data]` atau `[Nama]` |
| Opsional/kondisional | `(catatan)` |

> Fokus pada **hierarki dan posisi elemen**, bukan ukuran pixel. PM perlu memahami "apa ada di mana", bukan desain final.

---

## ✅ Checklist Sebelum Output ke PM

- [ ] Pre-Flight sudah dijalankan (AGENTS.md + ai-context dibaca)
- [ ] Semua view dari User Story terwakili di wireframe
- [ ] Desktop dan Mobile wireframe ada untuk setiap view
- [ ] Semua interaction states terdefinisi
- [ ] Component breakdown ada dengan status EXIST / NEW
- [ ] Out of Scope UI sudah didefinisikan
- [ ] Tidak ada asumsi layout yang belum dikonfirmasi PM

---

## 🛑 Mandatory Stop — Menunggu Review PM

Setelah dokumen selesai, agent WAJIB menulis pesan berikut dan BERHENTI:

```
✅ UI/UX Spec sudah selesai dibuat.

📄 File yang dibuat:
- docs/features/[modul]/[modul]-ui-spec.md

🖼️ Summary view yang dirancang:
- [Nama View 1]: [deskripsi singkat layout]
- [Nama View 2]: [deskripsi singkat layout]

🧩 Komponen baru yang perlu dibuat: [jumlah]
   - [NamaKomponen] — [fungsi singkat]

🔍 Mohon review wireframe, interaction states, dan component list di atas.
Jika sudah sesuai, ketik: "Lanjut Engineering Task"
Jika ada revisi, sebutkan view atau elemen yang perlu diubah.

⏸️ Saya menunggu konfirmasi sebelum membuat Engineering Task.
```
