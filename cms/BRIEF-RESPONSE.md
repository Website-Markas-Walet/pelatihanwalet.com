# Balasan Brief Integrasi CMS — pelatihanwalet.com

Situs ini adalah **kandidat pilot** (GitHub Pages, chrome seragam). Semua data di
bawah hasil pindai langsung repo. File pendukung ada di folder `cms/` yang sama.

---

## [BLOCKER #1] Mekanisme deploy — TERKONFIRMASI

- **Hosting:** GitHub Pages (custom domain via `CNAME` = `pelatihanwalet.com`).
- **Repo:** `Website-Markas-Walet/pelatihanwalet.com`
- **Branch produksi:** `main`
- **Alur repo → live:** `push ke main` = **langsung live**. TIDAK ada build step,
  TIDAK ada GitHub Actions. File di repo = file yang disajikan apa adanya.
- **Target tombol "Publish":** cukup **commit file ke `main`**. Tidak perlu deploy hook.
- **Catatan jebakan Jekyll:** repo ini SEBELUMNYA tanpa `.nojekyll`. Sudah saya
  tambahkan `.nojekyll` di root supaya file/direktori baru dari CMS (mis. yang
  berawalan `_`) tidak ditelan Jekyll. Aman untuk file existing.

## [BLOCKER #2] Akses tulis untuk CMS — REKOMENDASI

CMS commit langsung ke GitHub (bukan Cloudflare, jadi tidak ada Deploy Hook URL).
Urutan rekomendasi (paling aman dulu):

1. **GitHub App** (paling disarankan) — install ke repo ini saja, permission
   `Contents: Read & write`. Token auto-rotate, paling aman untuk multi-repo.
2. **Fine-grained PAT** — scope hanya repo ini, `Contents: write`. Simpan sebagai
   secret di Vercel (CMS), jangan di repo.
3. **Deploy key (SSH, write)** — kalau CMS `git push` biasa.

> Token TIDAK dikirim di sini. Pilih mekanisme (1/2/3), lalu owner repo yang
> generate & pasang di sisi CMS.

---

## Deliverable lain (sudah disiapkan di folder `cms/`)

| Poin brief | File | Isi |
|---|---|---|
| Chrome jadi template | `chrome-template.html` | header+footer existing → 1 layout, slot `<!-- CONTENT -->`, field SEO jadi `{{PLACEHOLDER}}` |
| url_patterns | `url-patterns.json` | pola per tipe + keanehan (.html vs folder, kota dobel-file) |
| Config situs | `site-config.json` | analytics, brand, menu utama, menu ekosistem, kontak |
| Ekspor artikel | `articles.json` | 12 artikel: title, slug, meta, canonical, tanggal, OG image |
| Dataset kota | `kota-dataset.json` | **1 template + 489 baris** (slug, daerah, provinsi) — bukan 489 halaman manual |
| Inventaris media | `media-inventory.json` | 203 file lokal (224MB) + 934 hotlink eksternal untuk rehome ke R2 |

### Catatan penting per poin
- **chrome-template.html** perlu **1x verifikasi render** sebelum dipakai produksi
  (diekstrak dari Elementor+Astra; slot sudah presisi di boundary `entry-content`).
- **url_patterns — keanehan:** tiap kota punya **2 file** (`kota/{slug}/index.html`
  DAN `kota/{slug}.html`). Artikel = `.html` di root; page = folder pretty-URL.
- **Dataset kota:** 34 provinsi lengkap; 9 baris nama daerah/provinsi kosong (perlu
  isi manual — slug tetap valid).
- **Hotlink berat:** 491 gambar dari `1.bp.blogspot.com` + goodnewsfromindonesia,
  medcom, ytimg, dll. Kandidat rehome ke `cdn.markaswalet.id` (R2).
- **URL/slug TIDAK berubah** → tidak perlu peta redirect 301.

---

## Ringkasan untuk CMS
> Situs statis GitHub Pages, `push ke main = live`, tanpa build. Bungkus halaman
> baru pakai `cms/chrome-template.html` (isi slot `<!-- CONTENT -->` + placeholder
> SEO). Konten existing sudah diekspor terstruktur di `cms/*.json`. URL dipertahankan
> apa adanya. Beri CMS akses tulis via GitHub App / fine-grained PAT ke repo ini.
