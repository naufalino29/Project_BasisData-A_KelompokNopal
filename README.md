# RentGo
Aplikasi Marketplace Untuk sewa kendaraan
## 1. Latar Belakang
Banyak pemilik kendaraan membiarkan kendaraanya mengganggur, sementara orang lain membutuhkan kendaraan untuk beberapa hari saja. Keduanya biasanya bertemu lewat group char atau media sosial, sehingga tarif, ketersediaan tanggal, dan riway

## 2. Tujuan
- Memungkinkan satu akun mendaftarkan mobil untuk disewakan sekaligus menyewa mobil milik akun lain.
- Menampilkan mobil yang tersedia pada rentang tanggal tertentu, dengan filter kota, kategori, dan tarif.
- Mencatat alur pemesanan dari pengajuan sampai selesai, beserta pembayaran dan biaya platform.
- Membangun kepercayaan antar pengguna lewat rating dan ulasan setelah sewa selesai.

## 3. Deskripsi Produk
**RentGo** adalah aplikasi marketplace penyewaan kendaraan terdepan yang menghubungkan pemilik kendaraan dengan pengguna yang membutuhkan moda transportasi dengan cepat, fleksibel, dan terpercaya. Mulai dari mobil keluarga untuk liburan, motor lincah untuk perjalanan harian, hingga kendaraan komersial untuk kebutuhan bisnis—semua ada di dalam satu genggaman!

## 4. Kebutuhan Pengguna

- Sebagai pengguna, Mendaftarkan satu akun untuk menyewakan sekaligus menyewa kendaraan agar tidak perlu mempunyai dua akun.
- Sebagai Pemilik, Mendaftarkan kendaraan lengkap dengan jenis, foto, tarif harian, dan kota agar kendaaraan bisa di temukan oleh pihak yang ingin menyewakan.
- Sebagai Pemilik, Menonaktifkan kendaraan untuk sementara agar tidak ada pesanan saat kendaraan ingin dipakai sendiri atau sedang tidak bisa untuk di sewakan.
- Sebagai Pemilik, Menerima atau menolak pengajuan pemesanan agar pemilik agar pemilik dapat menentukan siapa yang memakai kendaraan.
- Sebagai Penyewa, Mencari kendaraan berdasarkan jenis, kota, tarif, dan kata kunci agar penyewa cepat untuk menemukan kendaraan yang cocok.
- Sebagai penyewa, Memesan kendaraan pada tanggal tertentu dan melihat total harganya agar penyewa tahu total biaya sebelum membayar
- Sebagai Penyewa, Membayar pesanan yang sudah dikonfirmasi Agar pemilik yakin pesananya serius.
- Sebagai Penyewa, Memberi rating dan ulasan setelah sewa selesai Agar penyewa berikutnya memiliki acuan.

## 5. Ruang Lingkup
### Termasuk
- Operasi tambah, ubah, hapus, dan lihat data (CRUD) untuk pengguna, kendaraan, dan foto.
- Alur status pemesanan: menunggu, dikonfirmasi, berjalan, selesai, ditolak, dibatalkan.
- Pencarian kendaraan, pengecekan ketersediaan tanggal, dan ulasan.
- Wireframe antarmuka sederhana

### Tidak Termasuk
- Payment gateway, rekening penampung, dan pencairan dana ke pemilik.
- Verifikasi KTP dan SIM otomatis.
- Asuransi dan penanganan klaim kerusakan.
- Pelacakan posisi kendaraan (GPS) dan chat antar pengguna.
- Notifikasi otomatis dan aplikasi mobile native.

## 6. Persyaratan Fungsional
| Kode | Persyaratan | Entitas ERD |
|------|-------------|-------------|
| FR-01 | Sistem menyimpan registrasi pengguna; email dan NIK harus unik, dan satu akun berlaku untuk peran pemilik dan penyewa. | `pengguna` |
| FR-02 | Admin dapat mengelola kategori kendaraan (contoh: Mobil, Motor, Minibus); jenis baru ditambah sebagai baris data. | `kategori_kendaraan` |
| FR-03 | Pemilik dapat mendaftarkan kendaraan dengan plat nomor , merek, model, tarif harian, kota, dan kategori. | `kendaraan`, `kategori_kendaraan`, `pengguna` |
| FR-04 | Satu kendaraan dapat memiliki banyak foto. | `foto_kendaraan`, `kendaraan` |
| FR-05 | Pemilik dapat mengaktifkan atau menonaktifkan kendaraannya. | `kendaraan` |
| FR-06 | Penyewa dapat mencari kendaraan dengan `LIKE` pada merek atau model, dan memfilter berdasarkan kota dan kategori. | `kendaraan`, `kategori_kendaraan` |
| FR-07 | Penyewa dapat mengajukan pemesanan dengan memilih kendaraan dan tanggal; total harga = tarif harian x jumlah hari. | `pemesanan`, `kendaraan`, `pengguna` |
| FR-08 | Sistem menolak pemesanan yang tanggalnya tumpang tindih dengan pemesanan berstatus dikonfirmasi atau berjalan. | `pemesanan`, `kendaraan` |
| FR-09 | Pemilik dapat menerima atau menolak pemesanan; status berubah mengikuti alur pemesanan. | `pemesanan` |
| FR-10 | Penyewa dapat membayar pesanan yang sudah dikonfirmasi; satu pesanan boleh dibayar beberapa kali. | `pembayaran`, `pemesanan` |
| FR-11 | Penyewa memberi satu ulasan dengan rating 1 sampai 5 pada pemesanan yang selesai. | `ulasan`, `pemesanan` |
| FR-12 | Admin dapat melihat tarif harian termurah dan termahal. | `kendaraan` |
| FR-13 | Admin dapat melihat rating rata-rata tiap kendaraan. | `kendaraan`, `pemesanan`, `ulasan` |
| FR-14 | Menghapus kendaraan ikut menghapus fotonya, dan menghapus pemesanan ikut menghapus pembayaran dan ulasannya. | `kendaraan`, `foto_kendaraan`, `pemesanan`, `pembayaran`, `ulasan` |
| FR-15 | Pengguna dan kendaraan yang punya pemesanan tidak dapat dihapus (`ON DELETE RESTRICT`). | `pengguna`, `kendaraan`, `pemesanan` |

## 7. Persyaratan Non-Fungsional
- **konsistensi** Pemilik tidak dapat memesan kendaraannya sendiri, dan satu kendaraan tidak punya dua pemesanan aktif pada tanggal yang tumpang tindih; keduanya dijaga oleh trigger.
- **Performa** Pencarian kendaraan selesai di bawah 1 detik pada 10.000 baris kendaraan.
- **Keamanan** Kata sandi disimpan sebagai hash.

## 8. Resiko dan Asumsi
### Resiko
- Dua pemesanan aktif pada kendaraan dan tanggal yang sama sehingga kendaraan terjadwal ganda.
- Pemilik kendaraan memesan kendaraanya sendiri sehingga ulasan dan pendapatanya bersifat palsu.
- Tarif harian berubah sehingga Total pesanan lama ikut berubah.
- Pengguna tidak saling percaya karna platform hanya bersifat perantara sehingga pesanan bersifat sepi.

### Asumsi
- Platform hanya perantara serah terima dan pengembalian kendaraan dilakukan langsung antara pemilik dan penyewa.
- Jenis kendaraan hanya dibedakan lewat kategori; kolom lain (merek, model, tarif) berlaku sama untuk mobil, motor, dan minibus.
- Pembayaran dicatat sebagai bukti pembayaran penyewa; pencairan dana ke pemilik di luar lingkup.

## 9. Anggota Kelompok
| No | Nama | NPM|
| ---- | ---------- | ----- |
| 01 | Naufalino Ahmad Saputra | 4525210139 |
| 02 | M. Akbar Raffael Ridho | 4525210085 |