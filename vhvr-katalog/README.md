# VHVR Katalog — prototipe terpisah

Halaman ini dibuat di folder `vhvr-katalog/` pada branch `vhvr-katalog-prototype-2026-11-08`. File utama website lama `index.html` tidak diubah.

## Fitur prototipe
- Katalog contoh TikTok, Instagram, dan YouTube.
- Pencarian, filter platform, dan pengurutan harga.
- Panel admin lokal untuk menambah dan menghapus layanan.
- Data tersimpan di localStorage browser perangkat yang digunakan.
- Ringkasan pesanan simulasi yang dapat disalin.

## Batasan penting
- Semua nama layanan dan harga adalah DATA CONTOH, bukan penawaran penyedia.
- Tidak ada pembayaran, pemotongan saldo, pengiriman pesanan, ataupun koneksi Telegram/API penyedia.
- Panel admin belum memiliki autentikasi aman dan hanya untuk uji tampilan pada perangkat sendiri.
- Jangan memasukkan token, kata sandi, atau data pelanggan ke prototipe ini.

## Uji coba
Buka `vhvr-katalog/index.html` melalui GitHub Pages setelah branch/PR dipublikasikan atau jalankan file secara lokal. Perubahan pada katalog hanya tersimpan di browser yang sama.

## Tahap berikutnya
Temukan backend utama bot dan penyedia API yang sah. Setelah kredensial disimpan aman sebagai environment secrets di server, backend dapat menyediakan katalog nyata dan memproses pesanan. Jangan menaruh API key di HTML/browser.
