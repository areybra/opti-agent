# Scoring Rubric — Contoh Penilaian 5 Pilar

Gunakan sebagai kalibrasi Fase 1. Jangan invent skor tanpa quote.

---

## Contoh 1 — Buram (Skor 1/10) — BLOCK

**Prompt:** "buatkan landing page bagus"

| Pilar | Skor | Bukti Quote | Alasan |
|-------|------|-------------|--------|
| Role | 0 | — tidak ada — | Tanpa peran |
| Task | 1 | "buatkan landing page" | Ada verb tapi tanpa deliverable spesifik, lokasi, jumlah section |
| Context | 0 | — tidak ada — | Tanpa audiens, latar, produk |
| Constraints | 0 | — tidak ada — | Tanpa format, stack, tone |
| Iterate | 0 | — tidak ada — | Tanpa kriteria sukses |
| **Total** | **1** | | **BLOCK — 5 pertanyaan** |

---

## Contoh 2 — Cukup (Skor 6/10) — BLOCK

**Prompt:** "buatkan landing page SaaS untuk startup edukasi, pakai Next.js"

| Pilar | Skor | Bukti Quote | Alasan |
|-------|------|-------------|--------|
| Role | 0 | — tidak ada — | Tanpa peran |
| Task | 2 | "buatkan landing page SaaS" + "pakai Next.js" | Verb + produk + stack |
| Context | 1 | "untuk startup edukasi" | Ada konteks produk tapi tanpa audiens spesifik |
| Constraints | 1 | "pakai Next.js" | Ada stack tapi tanpa format, panjang, tone |
| Iterate | 0 | — tidak ada — | Tanpa kriteria sukses |
| **Total** | **4** | | **BLOCK — 4 pertanyaan** |

*Catatan: Meski total 4, tetap BLOCK karena Role 0 load-bearing untuk landing page.*

---

## Contoh 3 — Jelas (Skor 9/10) — PASS

**Prompt:** "Bertindak sebagai senior UI/UX + marketing-site-skill. Buat landing page SaaS B2B edukasi untuk decision maker kampus (rektorat). Stack Next.js 14 + Tailwind, 5 section (hero, fitur, harga, testimoni, CTA demo), tone premium editorial. Done jika Lighthouse >90 dan konversi demo +20%. Max 2 review loop."

| Pilar | Skor | Bukti Quote | Alasan |
|-------|------|-------------|--------|
| Role | 2 | "Bertindak sebagai senior UI/UX + marketing-site-skill" | Spesifik + domain |
| Task | 2 | "Buat landing page SaaS B2B edukasi" + "5 section" | Verb + deliverable + scope |
| Context | 2 | "untuk decision maker kampus (rektorat)" | Audiens + situasi nyata |
| Constraints | 2 | "Next.js 14 + Tailwind, 5 section, tone premium editorial" | Format + stack + tone |
| Iterate | 1 | "Done jika Lighthouse >90" + "Max 2 review loop" | Kriteria terukur tapi tanpa reviewer eksplisit |
| **Total** | **9** | | **PASS — tampilkan skor + konfirmasi singkat, boleh langsung eksekusi** |

---

## Kalibrasi Cepat

- Jika ragu antara 1 dan 2, pilih 1 — jangan over-score.
- Quote harus verbatim dari prompt, bukan parafrase.
- Jika prompt memakai bahasa Indonesia, quote dan alasan pakai Indonesia.
- Jika prompt English, pakai English.
