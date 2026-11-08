# Rencana integrasi backend VHVR BOT

Dokumen ini menetapkan batas aman sebelum katalog demo dihubungkan ke bot dan penyedia layanan.

## Status sekarang
- Katalog di `vhvr-katalog/index.html` adalah prototipe frontend.
- Data layanan dan harga adalah contoh.
- Penyimpanan memakai browser lokal; bukan database bersama.
- Tombol "Lihat ringkasan" hanya simulasi.
- Tidak ada request ke Telegram, payment gateway, atau provider API.

## Jangan lakukan
- Jangan menaruh bot token, provider API key, Supabase service-role key, atau rahasia lain di HTML/JavaScript browser.
- Jangan menerima pesanan berbayar berdasarkan harga contoh.
- Jangan menganggap pesanan selesai hanya karena browser menampilkan pesan sukses.
- Jangan mengubah webhook bot yang sedang aktif sebelum backend lamanya ditemukan dan ada rencana migrasi.

## Informasi yang diperlukan dari provider
Setelah panel provider ditemukan, catat tanpa membagikan rahasia:
1. Nama provider dan URL panel/API dokumentasi.
2. Jenis autentikasi (API key/token) — simpan hanya sebagai server secret.
3. Endpoint daftar layanan dan contoh respons yang sudah disamarkan.
4. Endpoint membuat pesanan dan parameter wajib.
5. Cara cek status pesanan, harga, minimum/maksimum, dan satuan biaya.
6. Kebijakan refill/refund/cancel serta batas penggunaan.

Jangan mengirim API key atau token ke chat. Jika bantuan diperlukan, kirim dokumentasi publik atau contoh respons yang sudah menghapus semua rahasia dan data pelanggan.

## Kontrak backend yang disarankan

Frontend hanya memanggil backend milik VHVR, bukan provider langsung:

- `GET /api/services?platform=tiktok&q=...`
  - Mengembalikan daftar layanan aktif yang sudah divalidasi server.
  - Field minimal: `id`, `name`, `platform`, `description`, `price_idr`, `min`, `max`, `active`.
- `POST /api/orders`
  - Memerlukan identitas Telegram yang diverifikasi server dari Telegram Mini App initData.
  - Memvalidasi ID layanan, jumlah, URL target, harga, saldo, dan idempotency key di server.
  - Membuat catatan pesanan internal sebelum meneruskan ke provider.
- `GET /api/orders/:id`
  - Hanya dapat diakses pemilik pesanan atau admin yang terautentikasi.
- `GET /api/admin/services` dan `POST/PATCH /api/admin/services`
  - Wajib autentikasi admin dan otorisasi server-side.
- Webhook/status provider
  - Diverifikasi sesuai mekanisme tanda tangan provider; jangan percaya status dari browser.

## Aturan transaksi
1. Harga final dihitung server, tidak pernah dipercaya dari browser.
2. Gunakan ID layanan internal dan simpan snapshot harga saat order dibuat.
3. Cegah order ganda dengan idempotency key.
4. Catat status: `pending`, `processing`, `completed`, `partial`, `failed`, `cancelled`, atau `refunded`.
5. Saldo dan ledger harus diperbarui dalam transaksi database yang konsisten.
6. Simpan audit log perubahan layanan, harga, saldo, dan status.
7. Jangan mengirim order ke provider sebelum validasi saldo dan transaksi internal berhasil.
8. Notifikasi Telegram hanya pemberitahuan; bukan sumber kebenaran status pembayaran/order.

## Checklist uji frontend
- Buka di layar ponsel.
- Cari kata "followers" dan pastikan hasil tersaring.
- Pilih tab TikTok, Instagram, dan YouTube.
- Urutkan harga rendah/tinggi.
- Tambah satu layanan demo di panel admin, lalu muat ulang halaman.
- Hapus layanan demo dan pulihkan data contoh.
- Pastikan ringkasan menyebut "simulasi" dan tidak mengirim pesanan.

## Gerbang rilis
Jangan merge/deploy ke produksi sampai:
- backend dan provider nyata sudah diketahui;
- autentikasi admin aman sudah tersedia;
- katalog nyata dan harga sudah diverifikasi;
- order, saldo, idempotensi, dan status diuji;
- bot lama dan alur notifikasi sudah dipetakan;
- pengguna menyetujui perubahan produksi.
