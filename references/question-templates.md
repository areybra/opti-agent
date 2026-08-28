# Question Templates — Prompt Refiner

Pilih 3-5 paling load-bearing. Prioritas: pilar skor 0 → 1. Jangan tanya yang sudah jelas. Ikuti bahasa input user.

## Aturan Umum

- Maks 5 pertanyaan per putaran.
- Setiap pertanyaan = Kategori pilar + Pertanyaan lugas + Kenapa penting (dampak jika tidak dijawab).
- Akhiri dengan: "Jawab nomor yang relevan (boleh sebagian), atau ketik 'pakai default'."
- Jika user jawab sebagian, isi sisanya dengan asumsi aman bertanda `TBD`/`unknown` dengan alasan.

---

## 1. Role Assignment

**ID:**
- "Peran spesifik apa yang Anda harapkan dari AI? Contoh: pakar pemasaran digital, senior backend architect, UX auditor, atau peran lain? — Kenapa penting: sudut pandang mengubah rekomendasi (marketing vs engineering)."
- "Level/sudut pandang yang diinginkan? Misal senior vs junior, B2B vs B2C? — Dampak: menentukan kedalaman dan istilah yang dipakai."

**EN:**
- "What specific role should the AI assume? e.g., digital marketing expert, senior backend architect, UX auditor? — Why: perspective changes the advice."
- "What seniority or lens do you want? Senior vs junior, B2B vs B2C? — Impact: depth and terminology."

## 2. Task Clarity

**ID:**
- "Deliverable akhirnya apa dan di mana? Contoh: file `app/page.tsx`, laporan markdown, atau refactor file existing? — Dampak: salah lokasi = kerja sia-sia."
- "Verb utamanya apa? Buat baru / refactor / audit / optimasi? — Dampak: 'percantik' bisa berarti redesign total atau ganti warna saja."
- "Berapa banyak varian/halaman yang diharapkan? — Dampak: 1 halaman vs 5 halaman beda effort 5x."

**EN:**
- "What is the final deliverable and where? e.g., file `app/page.tsx`, markdown report, refactor existing file? — Impact: wrong location = wasted work."
- "What is the main verb? Create / refactor / audit / optimize? — Impact: vague verbs cause random scope."

## 3. Context

**ID:**
- "Konteks bisnis/latar masalahnya apa? Kenapa ini dibutuhkan sekarang? — Dampak: tanpa why, solusi bisa miss kebutuhan."
- "Target audiens/penggunanya siapa? (umur, persona, B2B/B2C) — Dampak: copy dan UX untuk Gen Z vs enterprise beda total."
- "Ada data/asset/URL existing yang harus dipakai? — Dampak: jika ada design system atau URL lama, harus konsisten."

**EN:**
- "What is the business context / problem background? Why now? — Impact: without why, solution may miss the need."
- "Who is the target audience? — Impact: copy and UX differ drastically by persona."
- "Is there existing data/asset/URL to reuse? — Impact: design system or existing site constrains the solution."

## 4. Constraints

**ID:**
- "Format output yang diharapkan? (React/Next.js/Tailwind, markdown, JSON, panjang teks) — Dampak: format salah = tidak bisa dipakai."
- "Stack/teknologi/tone yang wajib atau yang harus dihindari? — Dampak: salah stack = rewrite total."
- "Batasan panjang/SEO/sumber? Misal max 500 kata, jangan pakai sumber X, harus mobile-first? — Dampak: tanpa batasan jawaban melebar."

**EN:**
- "Expected output format? (React/Next.js/Tailwind, markdown, JSON, text length) — Impact: wrong format = unusable."
- "Required stack/tech/tone or things to avoid? — Impact: wrong stack = full rewrite."
- "Length/SEO/source constraints? e.g., max 500 words, avoid source X, mobile-first required?"

## 5. Evaluate & Iterate

**ID:**
- "Kriteria suksesnya apa yang terukur? Misal Lighthouse >90, conversion naik, atau lolos editorial gate? — Dampak: tanpa kriteria, iterasi tidak ada ujung."
- "Siapa yang akan review dan berapa max loop revisi? — Dampak: mencegah revisi tanpa batas."
- "Apa yang terjadi jika hasil belum sesuai? Revisi parsial atau full rewrite? — Dampak: menentukan strategi iterasi."

**EN:**
- "What is the measurable success criterion? e.g., Lighthouse >90, pass editorial gate? — Impact: no criterion = endless iteration."
- "Who will review and what is max review loops? — Impact: prevents infinite revisions."

---

## Contoh Paket 3-5 Pertanyaan (ID)

**Kasus: "buatkan landing page bagus" (skor 2/10)**
1. [Task] Deliverable akhirnya halaman apa, di stack apa (Next.js/React/HTML), berapa section?
2. [Context] Untuk produk/bisnis apa dan audiens siapa?
3. [Constraints] Tone/style apa (premium/minimal/playful) dan hal yang harus dihindari?
4. [Role] Mau saya berperan sebagai UX designer, copywriter marketing, atau full-stack dev?
5. [Iterate] Kriteria suksesnya apa — conversion, Lighthouse, atau approval Anda?

**Kasus: "audit website saya https://example.com" (skor 5/10)**
1. [Task] Audit fokus ke apa — UX, SEO, performa, atau semua?
2. [Constraints] Ada kompetitor/pembanding atau batasan stack yang harus dihormati?
3. [Iterate] Output mau berupa laporan markdown, TASK-PLAN, atau langsung perbaikan kode?
