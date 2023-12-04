# **Modul Install Router OS Mikrotik**

## **Unduh Router OS Image:**

- Unduh gambar Router OS dari [https://download.mikrotik.com/routeros/6.49.10/mikrotik-6.49.10.iso](https://dl18.nesabamedia.net/windows/routeros/RouterOS.6.33.NesabaMedia.iso)

## **Unduh Winbox:**

- Ambil Winbox dari [https://mt.lv/winbox64](https://mt.lv/winbox64).

## **Instal Image Router OS di Virtual Box:**

- Buka Virtual Box.
- Pilih "New".
- Pilih file ISO Router OS yang telah diunduh.
- Pilih version Unknown/64 bit
- Sesuaikan pengaturan RAM sesuai kebutuhan 128 MB.
- Atur ukuran harddisk sesuai kebutuhan 256 MB.
- Klik "Finish" untuk menyelesaikan proses impor.

## **Mulai OS:**

- Pilih mesin virtual yang telah diimpor.
- Klik "Start" untuk memulai sistem operasi.

## **Instalasi Router OS:**

- Tekan tombol 'a' untuk memilih semua paket.
- Tekan tombol 'i' untuk memulai instalasi Router OS.

## **Eject (Unmount) File ISO:**

- Setelah instalasi selesai, klik kanan pada CD/DVD drive (bawah pojok kanan, urutan ke dua dari kiri, gambar cd/dvd) dan pilih "Eject".
- Jika muncul pop-up, pilih "Force unmount".

## **Konfigurasi Jaringan:**

- Tutup OS terlebih dahulu untuk mengkonfigurasi antarmuka jaringan.
- Buka pengaturan mesin virtual.
- Pilih "Network" dan sesuaikan "Adapter 1" dengan NAT.
- Sesuaikan "Adapter 2" dengan Internal Network. Isi kolom dibawahnya dengan "ether2-client"
- Sesuaikan "Adapter 3" dengan Host-Only.

## **Jalankan WinBox:**

- Setelah konfigurasi jaringan selesai, jalankan aplikasi WinBox untuk mengelola Router OS.
