# Persiapan

## 1. Membuat Topologi Baru

Berikut adalah topologi jaringan yang akan digunakan pada modul 3.

![image1.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image1.png?raw=true)

![image2.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image2.png?raw=true)



1. Hapus terlebih dahulu file UML yang tidak diperlukan bekas praktikum kemarin

   ```
   rm Dzul Vessa Alqis Walx Monx Aldx Ardx
   ```

   Himbauan!!!

   - Jangan coba-coba melakukan `rm *` karena akan menghapus semuanya, termasuk file jarkom. Jika file tersebut terhapus, segera hubungi asisten.

2. Sesuaikan script `topologi.sh` dengan gambar topologi di atas dengan tambahan ketentuan sebagai berikut:

   - Memori client **Walx**, **Monx**, dan **Zonx** adalah **64M**
   - Memori router **Dzul** adalah **256M** karena akan menjadi DHCP Server.
   - Memori server **Aldx** dan **Ardx** adalah **128M** karena akan menjadi DNS Server dan Proxy server.

3. Langkah-langkah selengkapnya silahkan mengikuti panduan membuat UML pada [Modul Pengenalan UML](https://github.com/aldonesia/ModulJarkomInformatikaITTS/tree/modul-1/modul1.5).



## 2. Konfigurasi Interface

Konfigurasi interface sama seperti [Modul Pengenalan UML](https://github.com/aldonesia/ModulJarkomInformatikaITTS/tree/modul-1/modul1.5), dengan tambahan:

- Zonx (Sebagai Client)

  ```
  auto eth0
  iface eth0 inet static
  address 192.168.0.4
  netmask 255.255.255.0
  gateway 192.168.0.1
  ```



## 3. Instalasi

Dalam modul 3, kita akan menggunakan 3 aplikasi, yaitu:

- **isc-dhcp-server** (DHCP Server)
- **squid3** (Proxy Server)
- **bind9** (DNS Server)

Lakukan langkah-langkah berikut:

1. Mengupdate package list pada Dzul, Aldx dan Ardx.

   ```
   apt-get update
   ```

2. Menginstal squid pada server Ardx

   ```
   apt-get install squid
   ```

3. Menginstal bind9 pada server Aldx

   ```
   apt-get install bind9
   ```



## Selamat Menyiapkan :)

n.b. Jika terjadi masalah, silahkan gugling. Jika masih juga belum menemukan solusinya, silahkan bertanya pada diri sendiri :3
