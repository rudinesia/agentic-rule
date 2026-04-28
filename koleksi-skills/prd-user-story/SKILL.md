---
name: prd-user-story
description: Skill untuk membuat PRD dan User Story sesuai format ARRUMI untuk fitur baru.
---

# PRD & User Story Skill

Gunakan skill ini untuk membuat dokumentasi kebutuhan produk (PRD) dan alur pengguna (User Story) terstandarisasi.

## 📂 Lokasi File & Penamaan
- **PRD**: `docs/features/[modul]/[modul]-prd.md`
- **User Story**: `docs/features/[modul]/[modul]-user-story.md`
- **Link**: WAJIB Relative Path (Skill `obsidian-markdown-links`).

## 🏗️ Template Standar ARRUMI

### 1. Product Requirements Document (PRD)
- **Ringkasan**: "Why" fitur ini dibangun.
- **Goals**: Business Goals vs User Goals.
- **FR (Functional)**: Must-Have / Should-Have + Acceptance Criteria.
- **NFR (Non-Functional)**: Page load < 2s, RLS isolation, Mobile responsive.
- **Data Model**: SQL Schema (Tabel baru/ubah) + Mermaid ERD.
- **Logic**: Edge Cases & Happy Path flows.
- **Success Metrics**: KPI (Load time, Success rate).

### 2. User Story (US)
Gunakan format **Verb-Noun** (`US-DASH-01: View Dashboard Stats`).
- **Story Statement**: "Sebagai [Role], Saya ingin [Fitur], Agar [Manfaat]".
- **AC (Gherkin)**: Scenario 1 (Happy), Scenario 2 (Validation Error).
- **Implementation**: Table Name, RLS Policy, Route, Components, State (React Query).
- **DoD (Definition of Done)**: Linter pass, RLS verified, Mobile-friendly.

## 🏷️ Kode Modul Standard
`AUTH` (Auth), `DASH` (Dash), `PROF` (Profil), `SDM` (SDM), `SAR` (Sarana), `KEU` (Keu), `KIN` (Kin), `ADM` (Admin), `RNST` (Renstra), `EPI` (Penyakit).

## ✅ Checklist Review
- [ ] Requirement atomic & testable.
- [ ] Edge cases (Offline, Conflict) terdefinisi.
- [ ] RLS Policy sudah spesifik (Tenant vs Global).
- [ ] Mobile behavior didefinisikan (Drawer vs Dialog).
- [ ] No absolute file paths (`C:/`, `file:///`).
