# Integrations

Daftar integrasi eksternal, statusnya, dan batasnya. Level otomasi mengikuti `clau-market/automations/` (saat ini **Level 1: manual**).

## Status Integrasi

| Sistem | Kegunaan | Arah | Status | Batas |
|---|---|---|---|---|
| AI model provider (Claude API, Anthropic) | Drafting, klasifikasi, first-pass QA | Platform (worker VPS 2; server dimatikan owner 2026-10-22, dibuat lagi saat ada dana) → provider | Antrean + worker + provider tiruan (backend 0.9.0); provider Claude menyusul (Tahap 2 PR 4c). Belum ada panggilan ke provider nyata | AI-BOUNDARY § Data ke Provider AI. Per environment dicatat di bawah |
| GitHub (repo CLAU) | Sumber rules/skills/knowledge | Platform ← repo | Planned (baca registry & skill) | Read-only |
| Instagram / Facebook (Meta) | Insight, posting konten yang disetujui klien | Platform → Meta Graph API (OAuth klien) | **Planned (model langganan v4.0.0)**: butuh Meta App Review + akun Business/Creator klien | Posting hanya setelah klien menyetujui di dashboard; token terenkripsi (AI-BOUNDARY § Token Kanal & Pembayaran) |
| TikTok | Insight, post | Platform → TikTok Content Posting API (OAuth klien) | **Planned**: butuh audit aplikasi TikTok | Sama seperti di atas |
| Google Business Profile | Post, balasan review | Platform → Business Profile API (OAuth klien) | **Planned** | Balasan review negatif tetap eskalasi ke klien (Level C) |
| OTA (Traveloka, tiket.com, Booking.com, Agoda, Airbnb) | Listing | — | Tidak diintegrasikan | Listing diupdate manusia di extranet |
| WhatsApp Business | Inquiry, broadcast | — | Tidak diintegrasikan (Pilar 2 AI Sales planned; Level A hanya via template disetujui client, AI-CONSTITUTION §4a) | Broadcast hanya ke kontak opt-in |
| PMS / booking engine | Ketersediaan, harga | — | Tidak diintegrasikan | AI tidak mengubah harga/ketersediaan |
| Payment gateway (Midtrans; Xendit sebagai cadangan) | Bayar paket & top up kredit (QRIS, VA, e-wallet) | Klien → gateway → webhook ke platform | **Planned (model langganan v4.0.0)**: sandbox dulu, produksi setelah akun merchant & legalitas usaha | Tidak menyimpan data kartu/rekening; status lunas hanya dari webhook terverifikasi (AI-BOUNDARY § Token Kanal & Pembayaran) |
| Provider gambar & video AI (Gemini image, Veo) | Agen gambar & video | Platform (worker) → provider | Planned | Sama dengan aturan provider AI; tarif kredit ≥ 3× biaya per aksi |

## Provider AI per Environment

| Environment | Provider | Model (arahan owner 2026-10-15) | Wilayah & masa simpan di provider | Status |
|---|---|---|---|---|
| Lokal / CI | Tiruan (`CLAU_AI_PROVIDER=fake`) | — | Tidak ada data keluar | Aktif |
| VPS 2 | Claude API (Anthropic) | Sulit: `claude-fable-5-1` · Utama: `claude-opus-5-5` · Ringan: `claude-haiku-5-5` (setelah lolos eval) | **NEXT ACTION:** dicatat saat akun provider dibuat (owner). `claude-fable-5-1` mensyaratkan retensi minimal 30 hari (tidak bisa zero data retention) | Belum (menunggu akun & biaya, TASK-052) |

## Aturan Menambah Integrasi

1. Tulis use case, data yang mengalir, dan zona penyimpanannya ([data-flow.md](./data-flow.md)).
2. Pastikan tidak ada aksi publish/kirim/ubah sistem client tanpa langkah approval manusia.
3. Credential lewat secret manager; scope akses minimum (read-only bila cukup).
4. Approval owner tertulis; update tabel ini dan [AI-BOUNDARY.md](../AI-BOUNDARY.md) bila ada zona baru.
5. Pemilihan vendor/tool dievaluasi tersendiri; dokumen ini tidak merekomendasikan merek.
