# Landing Page Skills

Tiga skill untuk membangun landing page yang berorientasi hasil bisnis: strategi dulu, lalu pola konversi saat mendesain, lalu standar teknis saat menulis kode. Isi skill berbahasa Inggris.

## Daftar Isi

- [Alur Kerja](#alur-kerja)
- [Daftar Skill](#daftar-skill)
- [Cara Pakai](#cara-pakai)
- [Struktur Folder](#struktur-folder)

## Alur Kerja

```
lp-strategy  →  lp-conversion  →  lp-technical
 (sebelum        (saat desain       (saat menulis
  desain/kode)    tiap section)      HTML/CSS/JS)
```

Strategi dijalankan **sebelum** desain atau kode apa pun. Dua skill lainnya berlaku pada setiap section dan setiap file yang dihasilkan, sehingga biasanya dipakai bersamaan saat build.

## Daftar Skill

| Skill | Kapan dipakai | Isi |
|-------|---------------|-----|
| [`lp-strategy`](lp-stategy/SKILL.md) | Sebelum menulis kode atau layout. Dipicu oleh "landing page", "hero section", "sales page", atau tujuan bisnis seperti "saya ingin lebih banyak klien". | Diagnosis satu tujuan bisnis (lead capture, penjualan langsung, authority/trust, atau filter klien), direktif "One Thing" (satu aksi utama), audit positioning, arsitektur situs dengan halaman sesedikit mungkin, aturan copy hero, dan output arsitektur. |
| [`lp-conversion`](lp-conversion/SKILL.md) | Saat mendesain atau mengkodekan section apa pun: hero, CTA, testimoni, pricing. | Blink Test 50ms, pengurangan beban kognitif (hierarki, navigasi, kepadatan copy), pola layout anti-grid, rekayasa CTA, hierarki social proof, resep micro-interaction, dan aturan konversi per section. |
| [`lp-technical`](lp-technical/SKILL.md) | Setiap kali menulis markup, CSS, atau JS untuk landing page. | Standar wajib: document shell (meta, OG, canonical), CSS mobile-first, Core Web Vitals, optimasi gambar, aksesibilitas, SEO + AIO, analytics, checklist performa, dan persyaratan handover ke klien. |

## Cara Pakai

Salin folder skill ke direktori skill agent Anda (misalnya `.claude/skills/`). Skill terpicu otomatis dari kata kunci di deskripsinya, atau Anda bisa menyebutnya langsung.

1. **Mulai dari strategi.** Minta agent membuat landing page. `lp-strategy` akan menanyakan tujuan bisnis utama lebih dulu, dan jika belum jelas, menentukannya menjadi hasil pertama sebelum ada halaman yang dibangun. Contoh:

   ```text
   PRIMARY ACTION: [verb] + [outcome] — mis. "Book a free strategy call"
   SECONDARY: none
   ```

2. **Desain dan tulis section** dengan `lp-conversion`. Hero harus lolos Blink Test, dan aturannya berlaku untuk semua section, bukan hanya hero.
3. **Bangun dengan standar teknis** dari `lp-technical`. Aturannya dianggap wajib untuk setiap file yang dihasilkan.

## Struktur Folder

```
landing-page/
├── lp-stategy/      # name di frontmatter: lp-strategy
├── lp-conversion/
└── lp-technical/
```

> Nama folder `lp-stategy` terlewat satu huruf (seharusnya `lp-strategy`), sedangkan `name` di dalam `SKILL.md` sudah benar. Jika agent memuat skill berdasarkan nama folder, samakan keduanya.
