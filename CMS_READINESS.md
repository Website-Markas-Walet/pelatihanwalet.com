---
site: pelatihanwalet.com
deploy: { host: github_pages, cf_account: null, branch: main, deploy_hook: absent }
url_patterns:
  artikel: "/{slug}.html (file .html di ROOT, bukan folder)"
  kota: "/kota/{slug} (DUPLIKAT: ada kota/{slug}/index.html DAN kota/{slug}.html)"
  page: "/{slug} (pretty-URL: {slug}/index.html)"
  desain: "/desain/{slug}.html (5 halaman, mis. rumah-burung-walet-ukuran-10x10-...)"
chrome:
  header_lines: "259-339 (index.html, blok id=masthead)"
  footer_lines: "1269-1347 (index.html, blok id=colophon)"
  byte_identical: "false — header:8 varian, footer:~513 varian; TAPI variansi benign (class active-state per-halaman + nonce WPForms). 1 kanonik dapat dipulihkan dengan strip active-state."
analytics:
  gtm: "GTM-MK5773B"
  ga4_property_id: ""
  tiktok: "CJA4IDBC77U5K7SPBH0G"
  fb_verify: "8fky9cw1ml1hqv0cwmu87qrtjk1rmy"
  adsense: ""
brand:
  primary: "#f45a2a"
  font: "Inter, Roboto"
  logo: "custom-logo (alt: 'Pelatihan Walet Icon')"
  favicon: "/wp-content/uploads/2023/02/cropped-icon-training-walet-1-32x32.png"
ecosystem_menu: present
contact:
  wa: "6287725260196"
  email: ""
media:
  local_mb: 224
  hotlink: "blogspot:491, goodnewsfromindonesia:51, medcom:48, ytimg:48, mediaindonesia:47, tagar:46, ekawalet:29, youtube:18 (total ~934)"
content:
  articles: 12
  kota_unique: 489
  kota_admin_level: kota
cleanup:
  - "Email kontak terkontaminasi: john@doe.com (demo Astra), sales@budidayawalet.net (situs lain), sales@markaswalet.com — perlu email kanonik pelatihanwalet"
  - "934 hotlink gambar eksternal (491 dari 1.bp.blogspot.com) → rehome ke R2/cdn.markaswalet.id"
  - "Tiap kota punya 2 file duplikat (kota/{slug}/index.html + kota/{slug}.html) — pilih 1 kanonik"
  - "9 halaman kota: nama daerah/provinsi kosong di <title>"
  - "Cruft WP-export statis: wp-admin/, wp-includes/, wp-json/, wp-login.php (mati; aman tapi noise 545MB repo)"
  - "Canonical homepage relatif ('/'), bukan absolut https://pelatihanwalet.com/"
blockers:
  - "Write access repo BELUM dibuka utk Claude GitHub App → push 403 'Resource not accessible by integration'. Commit siap di lokal, belum bisa dorong ke remote."
  - "Keputusan hosting media: uploads tetap di repo (224MB) atau pindah CDN; hotlink rehome"
  - "Email kontak kanonik yang benar (3 kandidat terkontaminasi)"
  - "Konfirmasi hanya GTM (tidak ada GA4/AdSense native) — tag lain mungkin dimuat di dalam GTM"
---

# Catatan — pelatihanwalet.com

**Ringkasan:** situs statis di GitHub Pages, `push ke main = live`, tanpa build/Actions/DB/API.
Satu-satunya jalur ubah konten = commit file ke `main`. Kandidat pilot (chrome nyaris seragam).

**Deploy & akses tulis (blocker):** target Publish cukup commit ke `main` — tidak ada deploy
hook. Rekomendasi akses tulis CMS: GitHub App scoped repo ini (`Contents: R/W`) → fine-grained
PAT (`Contents: write`, simpan di Vercel) → deploy key SSH. **Saat ini write masih 403**, jadi
izin App harus dibuka dulu.

**Chrome:** header (8 varian) & footer (~513 varian) beda hanya karena class active-state
per-halaman + nonce WPForms — bukan drift struktural. Template kanonik sudah diekstrak ke
`cms/chrome-template.html` (slot `<!-- CONTENT -->`, field SEO jadi `{{PLACEHOLDER}}`); perlu 1x
verifikasi render sebelum produksi.

**Konten & URL:** URL/slug tidak berubah → tidak perlu redirect 301. 489 kota = 1 template +
data (lihat `cms/kota-dataset.json`, 34 provinsi lengkap). Ekspor artikel, config situs, dan
inventaris media ada di folder `cms/*.json`.

**Jangan sentuh file situs:** file ini + folder `cms/` adalah satu-satunya tambahan; tidak ada
perubahan ke HTML/aset existing.
