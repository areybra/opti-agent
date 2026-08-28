# Refined Prompt — Template

> Salin block di bawah sebagai canonical prompt untuk eksekusi. Ikuti bahasa input user.

---

## Optimized Prompt (Siap Copy)

```
PERAN:
Bertindak sebagai [peran spesifik + level + domain]. Contoh: "pakar pemasaran digital B2B SaaS" / "senior UI/UX auditor" / "web security architect".

TUGAS:
[Verb spesifik + deliverable + lokasi]. Contoh: "Audit 3 URL (/, /harga, /blog) dan buat laporan markdown di reports/audit.md" / "Refactor komponen PricingCard di app/components/PricingCard.tsx".

KONTEKS:
Latar: [masalah / why sekarang]
Audiens: [siapa pengguna / persona]
Situasi/Data: [URL existing, asset, brand, data yang tersedia — atau "tidak ada, mulai dari nol"]

BATASAN:
- Format: [markdown / React + Tailwind / JSON / panjang teks]
- Stack/Teknologi: [Next.js 14, Tailwind, shadcn/ui — atau "bebas, rekomendasikan"]
- Tone/Style: [premium editorial / minimal / playful]
- Hal yang dihindari: [sumber, warna, pola, klaim yang dilarang]
- Batasan lain: [SEO, a11y, performa, budget]

EVALUASI & ITERASI:
- Kriteria sukses: [terukur — misal Lighthouse >90, pass editorial-quality-gate, 3 varian]
- Reviewer: [user / tim / automated check]
- Max loop: [misal 2]
- Aksi jika belum sesuai: [revisi parsial / full rewrite / tanya ulang]
```

---

## Diff Ringkas (vs Prompt Asli)

- Ditambah: [list apa yang diperjelas — misal Role, Context audiens, Constraints stack]
- Dipertegas: [verb kabur → verb spesifik]
- Asumsi aman: [jika ada yang tidak dijawab user, tulis di sini dengan tanda TBD — misal "Tone: TBD (asumsi premium editorial, konfirmasi?)"]

---

## Approval Gate

Pilih:
- **1. Pakai Refined** — lanjut eksekusi
- **2. Edit Manual** — revisi block di atas lalu konfirmasi
- **3. Tetap Original** — eksekusi dengan prompt asli skor [X/10] (risiko: hasil mungkin acak)

> Jangan lanjut eksekusi sebelum memilih 1/2/3.
