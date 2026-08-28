---
name: prompt-refiner
description: "Use when user prompt is vague, lacks role/task/context/constraints, or risks random execution. Audits prompt against 5 pillars (Role, Task, Context, Constraints, Iterate), scores clarity 0-10, blocks execution if <8, asks 3-5 targeted questions, and returns an optimized prompt before any task runs. Auto-invoked before task execution; follows input language."
---

# Prompt Refiner — Gate Anti Asal Eksekusi

Skill ini mencegah eksekusi buta. Setiap prompt minimal harus lulus 5 pilar sebelum agent menulis kode, membuat plan, atau memanggil tool berat.

## 5 Pilar Wajib

| # | Pilar | Pertanyaan Audit | Skor 0 | Skor 1 | Skor 2 |
|---|-------|------------------|--------|--------|--------|
| 1 | **Menentukan Peran (Role Assignment)** | Siapa AI ini? Identitas/profesi spesifik apa? | Tidak ada peran | Peran umum ("assistant") | Peran spesifik + sudut pandang ("pakar pemasaran digital B2B SaaS, fokus conversion") |
| 2 | **Kejelasan Tugas (Task Clarity)** | Apa instruksi utamanya? Verb + objek spesifik? | Umum/kabur ("buatkan yang bagus") | Ada verb tapi tanpa deliverable jelas | Verb lugas + deliverable + scope ("audit 3 URL dan buat TASK-PLAN.md v2") |
| 3 | **Pemberian Konteks (Context)** | Latar, audiens, situasi nyata? | Tanpa konteks | Sebagian ("untuk website") | Lengkap (latar masalah, audiens, tujuan bisnis, data yang ada) |
| 4 | **Menetapkan Batasan (Constraints)** | Koridor output? Format, panjang, stack, sumber yang dihindari? | Tanpa batasan | 1-2 batasan implisit | Format, panjang, stack, tone, hal yang dilarang eksplisit |
| 5 | **Evaluasi & Iterasi (Evaluate & Iterate)** | Kriteria sukses & loop perbaikan? | Tidak ada | Implisit ("nanti direview") | Kriteria sukses terukur + cara iterasi ("done jika Lighthouse >90, max 2 review loop") |

**Total 0-10.** Lihat `references/scoring-rubric.md` untuk contoh skor.

## Kapan Wajib Dipakai

### Must Use (BLOCK)
- Prompt skor total **< 8** atau **ada pilar = 0**
- Prompt tanpa verb spesifik, tanpa konteks, atau tanpa batasan yang menyebabkan ruang jawaban terlalu luas
- Task akan mengubah file, deploy, atau memanggil agent lain (biaya tinggi jika salah)
- User memakai bahasa kabur: "tolong benerin", "bikin bagus", "yang terbaik", "terserah"

### Recommended
- Prompt skor 8-9 — saring 1-2 pertanyaan presisi sebelum eksekusi
- Prompt multi-tahap tanpa urutan yang jelas

### Skip
- Prompt skor 10/10 (5 pilar lengkap, tidak ambigu)
- Pertanyaan faktual sederhana yang tidak butuh eksekusi ("apa itu X?")
- User eksplisit bilang `skip prompt-refiner` / `langsung eksekusi`

**Aturan Auto-Invoke:** Agent yang menerima prompt baru **WAJIB** menjalankan Fase 1 (Parse & Score) sebelum `Task`, `Write`, `Edit`, atau `Bash` destruktif. Jika skor < 8, agent **DILARANG** lanjut ke eksekusi — harus masuk Fase 2 (tanya) dan Fase 3 (rewrite).

**Bahasa:** Ikuti bahasa input user. Jika user pakai Indonesia, audit, pertanyaan, dan refined prompt pakai Indonesia. Jika English, pakai English. Jangan campur tanpa alasan.

## Workflow 4 Fase (BLOCK Gate)

### Fase 1 — Parse & Score (Wajib, < 30 detik)

1. Baca `references/5-pillars-checklist.md`.
2. Ekstrak 5 pilar dari prompt asli (quote bukti per pilar).
3. Beri skor 0/1/2 per pilar + total.
4. Tentukan gate:
   - `0-4 Buram` → BLOCK, butuh 5 pertanyaan
   - `5-7 Cukup` → BLOCK, butuh 3-4 pertanyaan
   - `8-10 Jelas` → PASS (tampilkan ringkasan skor + refined prompt ringan, minta konfirmasi singkat)

Output Fase 1 harus tabel skor + alasan per pilar + quote sumber. Jangan invent skor tanpa bukti.

### Fase 2 — Clarifying Questions (Hanya jika BLOCK, 3-5 pertanyaan)

1. Baca `references/question-templates.md`.
2. Pilih **3-5 pertanyaan** paling load-bearing (prioritas: pilar skor 0 → 1 → 2). Jangan tanya hal yang sudah jelas di prompt.
3. Setiap pertanyaan harus punya: kategori pilar, pertanyaan lugas, dan **kenapa ditanya** (dampak jika tidak dijawab).
4. Gunakan `default.question` tool jika tersedia untuk interaksi terstruktur; fallback ke list numbered jika tidak.
5. Akhiri dengan instruksi: "Jawab nomor yang relevan (boleh sebagian), atau ketik 'pakai default' untuk lanjut dengan asumsi aman."

**Anti-pattern:** Jangan tanya >5, jangan tanya hal generik yang tidak mengubah eksekusi, jangan tanya untuk formalitas.

### Fase 3 — Rewrite (Optimized Prompt)

Setelah user menjawab (atau jika Fase 1 PASS), hasilkan **Optimized Prompt** pakai `assets/refined-prompt.template.md`:

```
PERAN: Bertindak sebagai [peran spesifik + senioritas + sudut pandang]
TUGAS: [verb spesifik + deliverable + lokasi file jika ada]
KONTEKS: [latar masalah, audiens, situasi nyata, data/artifak yang tersedia]
BATASAN: [format output, panjang, stack, tone, sumber/ruang yang dihindari]
EVALUASI: [kriteria sukses terukur + cara iterasi + max review loop]
```

Sertakan juga:
- `Diff ringkas`: apa yang ditambah/diperjelas vs prompt asli
- `Asumsi aman` (jika user skip jawab): tulis eksplisit dan tandai `TBD` jika kritis

### Fase 4 — Approval Gate (BLOCK sampai dipilih)

Tampilkan berdampingan:

```
Prompt Asli: "..."
Skor: X/10 (R:X T:X C:X B:X E:X)

Prompt Teroptimasi:
[block siap copy]
```

Berikan 3 opsi:
- **1. Pakai Refined** — lanjut eksekusi dengan prompt teroptimasi
- **2. Edit Manual** — user revisi refined prompt
- **3. Tetap Original** — paksa pakai prompt asli (agent harus catat risiko: "dieksekusi dengan prompt skor X/10, hasil mungkin acak")

**JANGAN lanjut ke implementasi sebelum user memilih 1/2/3.** Jika user memilih 3 dengan skor <8, agent harus menambahkan warning di log dan tetap eksekusi sesuai instruksi eksplisit.

## Non-Negotiables

- Jangan eksekusi buta dengan skor <8 tanpa Fase 2+3.
- Jangan invent konteks/audiens/stack yang tidak ada di prompt atau jawaban user — tandai `TBD` atau `unknown`.
- Jangan tanya >5 pertanyaan dalam satu putaran.
- Jangan rewrite tanpa menampilkan skor dan bukti quote.
- Jangan simpan refined prompt ke file kecuali user meminta eksplisit (sesuai konfigurasi: tidak disimpan).
- Jangan ubah bahasa output dari bahasa input.
- Ikuti `BLOCK` gate — planned ≠ executed.

## Output Shape

1. **Tabel Skor 5 Pilar** (pilar, skor, bukti quote, alasan)
2. **Gate Decision**: `PASS` atau `BLOCK` + threshold
3. **Jika BLOCK**: 3-5 pertanyaan terstruktur (pilar + pertanyaan + kenapa penting)
4. **Optimized Prompt** (block siap copy, 5 pilar terisi)
5. **Diff & Asumsi** (apa yang ditambah, asumsi `TBD` jika ada)
6. **Approval Gate** (opsi 1/2/3, tunggu pilihan user)

Gunakan `assets/refined-prompt.template.md` untuk konsistensi.

## Validasi

```bash
# Cek struktur skill
ls -R ~/.config/opencode/skills/prompt-refiner

# Uji manual dengan 3 fixture
# buram: "buatkan landing page bagus"
# sedang: "buatkan landing page SaaS untuk startup edukasi, pakai Next.js"
# jelas: "Bertindak sebagai senior UX + marketing-site-skill. Buat landing page SaaS B2B edukasi... stack Next.js 14 + Tailwind, 5 section, CTA demo, tone premium editorial, done jika Lighthouse >90..."
```

## Handoff ke Eksekusi

Setelah user pilih `1 Pakai Refined` atau `2 Edit Manual (final)`, agent harus:
- Set `refined_prompt` sebagai canonical input untuk task berikutnya
- Cantumkan skor awal dan diff di ringkasan handoff
- Lanjut ke skill/plan/implementasi yang diminta user dengan konteks yang sudah diperjelas
