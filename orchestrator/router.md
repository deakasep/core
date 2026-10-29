# Router

Tabel routing request → pilar → skill → level otonomi (AI-CONSTITUTION §4a). Kolom **intent** dipakai juga oleh platform clau-agent (`agents/registry.json` → field `intents`).

## Tabel Routing

| Intent | Contoh request | Pilar | Skill / prompt | Repo | Approval minimum | Otonomi |
|---|---|---|---|---|---|---|
| `caption` | "Bikin caption IG untuk room tour" | 1 AI Marketing | `draft-caption` | clau-market | L1–L2 | B |
| `reel-script` | "Script reels 20 detik pagi di villa" | 1 AI Marketing | `draft-reel-script` | clau-market | L2 | B |
| `stories` | "Story 3 frame menu hari ini" | 1 AI Marketing | `draft-stories` (+ prompt `stories-sequence`) | clau-market | L1–L2 | B |
| `ota-listing` | "Update deskripsi Booking.com" | 1 AI Marketing | `write-ota-listing` | clau-market | L2 | B |
| `review-reply` | "Balas review Google bintang 2" | 1 AI Marketing* | `reply-guest-review` | clau-market | L1 (positif) / L3 (negatif; rating ≤ 3 otomatis L3, API 0.10.0) | B |
| `content-calendar` | "Kalender konten bulan depan" | 1 AI Marketing | `build-content-calendar` | clau-market | L1 | B |
| `seasonal-campaign` | "Promo Ramadan & Lebaran" | 1 AI Marketing | `plan-seasonal-campaign` | clau-market | L2–L3 | B |
| `localize` | "Terjemahkan caption ke EN" | 1 AI Marketing | `localize-content` | clau-market | Mengikuti konten asal (draft asal wajib dirujuk, API 0.10.0) | B |
| `inquiry-reply` | "Balas DM tanya harga & tanggal" | 2 AI Sales | — | — | **Planned → eskalasi** | B |
| `guest-message` | "Pesan pre-arrival ke tamu" | 2 AI Sales | — | — | **Planned → eskalasi** | B |
| `sop` | "SOP housekeeping" | 3 AI Admin | — | — | **Planned → eskalasi** | B |
| `report` | "Report performa bulanan" | 4 AI Business Intelligence | `build-monthly-report` (+ template `monthly-report`, Pilar 1) | clau-market | L1 | B |
| `image` | "Gambar promo diskon akhir pekan" | 1 AI Marketing | — | — | **Planned → eskalasi** | B |
| `video` | "Video AI 8 detik produk baru" | 1 AI Marketing | — | — | **Planned → eskalasi** | B |
| `data-analysis` | "Analisis file penjualan Oktober" | 4 AI Business Intelligence | — | — | **Planned → eskalasi** | B |
| `data-annotation` | "Labeli 500 ulasan positif/negatif" | 4 AI Business Intelligence | — | — | **Planned → eskalasi** | B |
| `integration` | "Hubungkan pesanan toko ke Google Sheets" | 5 AI Automation | — | — | **Planned → eskalasi** | B |
| `web-dev` | "Landing page promo sederhana" | 5 AI Automation | — | — | **Planned → eskalasi** | B |

\* `review-reply` sementara di Pilar 1. Saat Pilar 2 AI Sales aktif (customer service & reputasi), intent ini dipindah (**NEXT ACTION** saat aktivasi Pilar 2).

Intent `image`, `video`, `data-analysis`, `data-annotation`, `integration`, dan `web-dev` ditambahkan untuk tim agen langganan (AI-CONSTITUTION v4.0.0, 2026-10-29). Statusnya **Planned** sampai skill, persona, eval, dan tarif kreditnya ada; Pilar 5 AI Automation memakai `integration` dan `web-dev`, dan alur end-to-end tetap memanggil intent pilar lain (lihat [blueprints/automation.md](../docs/blueprints/automation.md)).

## Aturan

- Intent tidak dikenal → tanyakan klarifikasi ke requester; jangan menebak pilar.
- Request yang mengandung beberapa intent (mis. "caption + balas DM harga") dipecah per intent.
- Intent di pilar *Planned* tidak dikerjakan; buat kartu di board ClickUp (list pilar itu) dan eskalasi ke owner.
- Level approval di tabel adalah **minimum**; rules pilar bisa menaikkannya (mis. ada harga/promo → minimal L2). Di platform, kenaikan dari isi konten dihitung lewat penanda isi konten (`shared/api-contracts.md` § Penanda Isi Konten).
- Kolom **Otonomi** = level maksimum yang boleh dipakai intent itu (AI-CONSTITUTION §4a). Default semua intent **B**. Mengubah ke **A** butuh perintah owner, template Level A yang disetujui per client, audit log, dan tombol berhenti; level ini juga dicerminkan di `clau-agent/agents/registry.json` → `autonomy_max`.
- Apa pun levelnya, situasi Level C (komplain serius, refund, hukum, kesehatan, keselamatan, data sensitif) selalu eskalasi ke owner.
