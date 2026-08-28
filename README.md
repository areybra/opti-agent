# Prompt Refiner — Universal Agent Skill

> Gate anti asal eksekusi. Audit prompt dengan 5 pilar sebelum agent menulis kode.
> **Works with ALL agents:** OpenCode, Claude Code, Codex, Cursor, Windsurf, Copilot, Gemini CLI, ChatGPT, Perplexity, dan LLM apa pun.

Skill ini mencegah agent mengeksekusi prompt yang kabur. Setiap prompt diaudit terhadap 5 pilar — **Role, Task, Context, Constraints, Evaluate & Iterate** — diberi skor 0–10, dan **diblokir (BLOCK)** jika skor < 8 sampai diperjelas dengan 3–5 pertanyaan terarah.

**Bahasa:** Mengikuti bahasa input user (ID/EN otomatis).

## Universal vs OpenCode

| File | Untuk | Cara Pakai |
|------|-------|------------|
| `UNIVERSAL.md` | **Semua AI** (ChatGPT, Claude, Gemini, Cursor, Copilot, dll) | Copy-paste sebagai System Prompt / Custom Instruction |
| `SKILL.md` + `references/` + `assets/` | OpenCode | `cp -r` ke `~/.config/opencode/skills/prompt-refiner` |
| `AGENTS.md` | Codex, Cursor, Windsurf | Auto-read oleh agent |
| `CLAUDE.md` | Claude Code | Auto-read / paste ke Project Knowledge |

---

## 5 Pilar

| # | Pilar | Contoh Skor 2 |
|---|-------|---------------|
| 1 | **Role Assignment** | `Bertindak sebagai pakar pemasaran digital B2B SaaS` |
| 2 | **Task Clarity** | `Audit 3 URL dan buat TASK-PLAN.md v2` (verb + deliverable + lokasi) |
| 3 | **Context** | `Untuk decision maker kampus, latar: konversi demo rendah` |
| 4 | **Constraints** | `Next.js 14 + Tailwind, 5 section, tone premium editorial, jangan pakai emoji` |
| 5 | **Evaluate & Iterate** | `Done jika Lighthouse >90, max 2 review loop` |

## Cara Kerja (4 Fase)

1. **Parse & Score** — Ekstrak 5 pilar, skor 0/1/2 per pilar, total 0–10. Tampilkan bukti quote.
2. **Clarifying Questions** — Jika BLOCK (<8), tanya 3–5 pertanyaan paling load-bearing (prioritas pilar skor 0).
3. **Rewrite** — Hasilkan `Optimized Prompt` (PERAN/TUGAS/KONTEKS/BATASAN/EVALUASI) siap copy.
4. **Approval Gate** — User pilih: `1 Pakai Refined / 2 Edit Manual / 3 Tetap Original`.

```
Prompt Asli: "buatkan landing page bagus" → Skor 1/10 → BLOCK → 5 pertanyaan
Prompt Jelas: Role+Task+Context+Constraints+Iterate lengkap → Skor 9/10 → PASS
```

## Instalasi

### Universal — Untuk ChatGPT / Claude / Gemini / Cursor / Copilot (1 copy-paste)

1. Buka `UNIVERSAL.md` di repo ini
2. Copy seluruh isinya
3. Paste sebagai:
   - **ChatGPT:** Settings → Custom Instructions / GPT Builder → Instructions
   - **Claude:** Project Knowledge / `CLAUDE.md` di root project
   - **Cursor / Windsurf / Codex:** `AGENTS.md` di root project (sudah tersedia, tinggal copy file ini)
   - **Gemini:** Gems → Instructions
   - **Copilot:** `.github/copilot-instructions.md`

Tidak butuh tools, hanya markdown.

### OpenCode — 3 Opsi

**Opsi A — Copy Manual (paling mudah)**

```bash
# 1. Clone repo ini
git clone https://github.com/areybra/opti-agent.git
# atau jika folder bentrok:
git clone https://github.com/areybra/opti-agent.git prompt-refiner

# 2. Copy ke folder skills OpenCode
mkdir -p ~/.config/opencode/skills
cp -r opti-agent ~/.config/opencode/skills/prompt-refiner
# jika clone sebagai prompt-refiner:
# cp -r prompt-refiner ~/.config/opencode/skills/

# 3. Verifikasi
ls -R ~/.config/opencode/skills/prompt-refiner
# harus ada: SKILL.md, references/, assets/
```

**Opsi B — Symlink (dev)**

```bash
git clone https://github.com/areybra/opti-agent.git ~/opti-agent
ln -s ~/opti-agent ~/.config/opencode/skills/prompt-refiner
```

**Opsi C — Project-local**

```bash
mkdir -p .opencode/skills
cp -r /path/to/opti-agent .opencode/skills/prompt-refiner
```

> Restart OpenCode TUI setelah install. Skill akan terdeteksi otomatis.

### Cursor / Codex / Windsurf — AGENTS.md

```bash
cp AGENTS.md /path/to/your-project/AGENTS.md
# atau
cp UNIVERSAL.md /path/to/your-project/AGENTS.md
```

## Penggunaan

### Auto-Invoke (default)

Skill ini dirancang untuk **otomatis terpanggil** sebelum agent menjalankan `Task`, `Write`, `Edit`, atau `Bash` destruktif. Agent wajib menjalankan Fase 1 (Parse & Score) terlebih dahulu. Jika skor < 8, eksekusi diblokir sampai Fase 2–4 selesai.

### Manual

```
/prompt-refiner
```

Atau di chat:

```
Load skill prompt-refiner dan audit prompt berikut: "buatkan dashboard admin"
```

## Struktur

```
opti-agent/
├── UNIVERSAL.md                          # ← untuk SEMUA AI (1 file, copy-paste jadi system prompt)
├── AGENTS.md                             # auto-read Codex/Cursor/Windsurf
├── CLAUDE.md                             # untuk Claude Code
├── SKILL.md                              # workflow OpenCode (BLOCK gate)
├── references/
│   ├── 5-pillars-checklist.md            # rubrik skor 0/1/2
│   ├── question-templates.md             # template 3-5 pertanyaan ID/EN
│   └── scoring-rubric.md                 # contoh 1/10 vs 9/10
├── assets/
│   └── refined-prompt.template.md        # template Optimized Prompt
├── LICENSE
└── README.md
```

## Konfigurasi

Default skill ini:
- **Bahasa:** ikut input user
- **Gate:** BLOCK jika < 8
- **Pertanyaan:** 3–5 (tidak lebih)
- **Simpan file:** tidak (hanya tampilkan di chat)

Ubah di `SKILL.md` jika perlu.

## Contoh

**Input buram:**
```
buatkan landing page bagus
```

**Output skill:**
```
Skor: 1/10 (R:0 T:1 C:0 B:0 E:0) → BLOCK
Pertanyaan:
1. [Task] Deliverable akhirnya halaman apa, di stack apa, berapa section?
2. [Context] Untuk produk/bisnis apa dan audiens siapa?
3. [Constraints] Tone/style apa dan hal yang harus dihindari?
...

Optimized Prompt:
PERAN: Bertindak sebagai senior UI/UX + marketing-site-skill
TUGAS: Buat landing page ...
KONTEKS: ...
BATASAN: Next.js 14 + Tailwind ...
EVALUASI: Done jika ...

[1] Pakai Refined  [2] Edit Manual  [3] Tetap Original
```

## Lisensi

MIT — bebas pakai, modifikasi, distribusi.

## Kontribusi

PR welcome. Pastikan `SKILL.md` tetap konsisten dengan `references/` dan `assets/`.

---

**Dibuat untuk semua agent — OpenCode, Claude, Codex, Cursor, Gemini, ChatGPT. Satu skill, semua platform.**
