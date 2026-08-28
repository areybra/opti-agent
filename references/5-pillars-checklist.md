# 5 Pillars Checklist — Prompt Refiner

Gunakan checklist ini di Fase 1. Skor 0/1/2 per pilar. Wajib sertakan quote bukti.

## 1. Menentukan Peran (Role Assignment)
**Tujuan:** Memberi identitas spesifik agar sudut pandang jawaban presisi.

- [ ] Apakah ada peran/profesi eksplisit? ("pakar pemasaran digital", "senior security architect", "UX auditor")
- [ ] Apakah ada level senioritas / spesialisasi? ("senior", "B2B SaaS", "fintech")
- [ ] Apakah ada sudut pandang / domain yang diinginkan? ("dari sisi conversion", "dari sisi OWASP ASVS")

Skor:
- 0 = Tidak ada peran sama sekali
- 1 = Ada peran tapi generik ("AI assistant", "helper", "expert")
- 2 = Peran spesifik + konteks domain

Quote bukti wajib: salin frasa peran dari prompt atau tulis `— tidak ada —`.

## 2. Kejelasan Tugas (Task Clarity)
**Tujuan:** Instruksi lugas, verb spesifik, deliverable terukur.

- [ ] Apakah ada verb spesifik? (build, audit, refactor, buat, analisis — bukan "benerin", "percantik")
- [ ] Apakah ada objek/deliverable jelas? (file, halaman, komponen, laporan)
- [ ] Apakah ada scope/lokasi? (path file, URL, stack, jumlah section)

Skor:
- 0 = Kabur ("buatkan yang bagus", "tolong perbaiki")
- 1 = Ada verb tapi deliverable/scope kabur
- 2 = Verb + deliverable + scope/lokasi spesifik

## 3. Pemberian Konteks (Context)
**Tujuan:** Latar belakang, audiens, situasi nyata.

- [ ] Latar masalah / why? (kenapa butuh, problem apa yang diselesaikan)
- [ ] Target audiens / pengguna?
- [ ] Situasi nyata / data yang ada? (URL existing, asset, brand, constraint bisnis)

Skor:
- 0 = Tanpa konteks
- 1 = Sebagian ("untuk website", "untuk UMKM")
- 2 = Lengkap (latar + audiens + situasi/data)

## 4. Menetapkan Batasan (Constraints)
**Tujuan:** Koridor agar jawaban tidak melebar acak.

- [ ] Format output? (markdown, JSON, React component, panjang teks)
- [ ] Stack / teknologi / tone? (Next.js, Tailwind, formal/casual)
- [ ] Hal yang harus dihindari / sumber yang dilarang?

Skor:
- 0 = Tanpa batasan
- 1 = 1-2 batasan implisit
- 2 = Batasan eksplisit (format + panjang/stack + don'ts)

## 5. Evaluasi dan Iterasi (Evaluate & Iterate)
**Tujuan:** Kriteria sukses & loop perbaikan.

- [ ] Kriteria sukses terukur? (Lighthouse >90, pass editorial gate, 3 varian)
- [ ] Cara evaluasi / siapa reviewer?
- [ ] Batas iterasi / max review loop?

Skor:
- 0 = Tidak ada
- 1 = Implisit ("nanti direview", "sampai bagus")
- 2 = Kriteria terukur + proses iterasi eksplisit

## Gate Threshold

| Total | Gate | Aksi |
|-------|------|------|
| 0-4 | BLOCK Buram | 5 pertanyaan (prioritas pilar 0) |
| 5-7 | BLOCK Cukup | 3-4 pertanyaan (pilar 0 dan 1) |
| 8-10 | PASS Jelas | Tampilkan skor + refined ringan, konfirmasi singkat |

**Aturan Kritis:** Jika **ada satu pilar = 0** dan pilar itu load-bearing untuk task (misal Role untuk task audit, Constraints untuk task code), tetap BLOCK meski total 8.

## Cara Menilai

1. Untuk setiap pilar, cari quote langsung dari prompt (jangan inferensi).
2. Jika tidak ada quote, skor 0 dan tulis `— tidak ada —`.
3. Jumlahkan total.
4. Tentukan gate.
5. Lanjut ke `question-templates.md` untuk menyusun pertanyaan.
