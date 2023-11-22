# 2. Proxy Server

## 2.1 Pengertian, Fungsi, dan Manfaat

### 2.1.1 Pengertian

Proxy server adalah sebuah server atau program komputer yang berperan sebagai penghubung antara suatu komputer dengan jaringan internet. Atau dalam kata lain, proxy server adalah suatu jaringan yang menjadi perantara antara jaringan lokal dan jaringan internet.

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image32.jpeg?raw=true)

Proxy server dapat berupa suatu sistem komputer ataupun sebuah aplikasi yang bertugas menjadi gateway atau pintu masuk yang menghubungan komputer kita dengan jaringan luar.

### 2.1.2 Fungsi

1. ***Connection sharing*** : Proxy bertindak sebagai gateway yang menjadi pembatas antara jaringan lokal dengan jaringan luar. Gateway bertindak juga sebagai sebuah titik dimana sejumlah koneksi dari pengguna lokal dan koneksi jaringan luar juga terhubung kepadanya. Oleh sebab itu, koneksi dari jaringan lokal ke internet akan menggunakan sambungan yang dimiliki oleh gateway secara bersama-sama (connection sharing).
2. ***Filtering*** : Proxy bisa difungsikan untuk bekerja pada layar aplikasi dengan demikian maka dia bisa berfungsi sebagai firewalll paket filtering yang dapat digunakan untuk melindungi jaringan lokal terhadap gangguan maupun ancaman serangan dari jaringan luar. Fungsi filtering ini juga dapat diatur atau dikonfigurasi untuk menolak akses terhadap situs web tertentu dan pada waktu- waktu tertentu juga.
3. ***Caching*** : Sebuah proxy server mempunyai mekanisme penyimpanan obyek-obyek yang telah diminta dari server-server yang ada di internet. Dengan mekanisme caching ini maka akan menyimpan objek-objek yang merupakan berbagai permintaan/request dari para pengguna yang di peroleh dari internet.

### 2.1.3 Manfaat

Proxy server memiliki manfaat-manfaat berikut ini:

- Membagi koneksi
- Menyembunyikan IP
- Memblokir situs yang tidak diinginkan
- Mengakses situs yang telah diblokir
- Mengatur bandwith

### 2.1.4 Software Proxy Server

Beberapa contoh software proxy server yang sering digunakan adalah sebagai berikut:

1. CCProxy
2. WinGate
3. Squid
4. Nginx

### 2.1.5 Cara Kerja Squid

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image33.gif?raw=true)

## 2.2 Implementasi

Untuk praktikum jarkom kali ini, software proxy server yang digunakan adalah **Squid** dan UML yang digunakan sebagai proxy server adalah **Ardx**.

### 2.2.1 Instalasi Squid

**STEP 1** - Install squid pada UML **Ardx**, ketikkan:

```
apt-get install squid
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image34.png?raw=true)

**STEP 2** - Cek status squid untuk memastikan bahwa Squid telah berjalan dengan baik dengan mengetikkan

```
service squid status
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image35.png?raw=true)

Jika muncul status **ok** maka instalasi telah berhasil.

### 2.2.2 Konfigurasi Dasar Squid

**STEP 1** - Backup terlebih dahulu file konfigurasi default yang disediakan squid. Ketikkan perintah berikut untuk melakukan backup:

```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image36.png?raw=true)

**STEP 2** - Buat konfigurasi baru dengan mengetikkan:

```
nano /etc/squid/squid.conf
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image37.png?raw=true)

**STEP 3** - Kemudian, pada file config yang baru, ketikkan script:

```
http_port 8080
visible_hostname Ardx
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image38.png?raw=true)

**Keterangan:**

- `http_port 8080` : Port yang digunakan untuk mengakses proxy, dalam kasus ini adalah **8080**. (Sintaks: `http_port 'PORT_YANG_DIINGINKAN'`)
- `visible_hostname Ardx` : Nama proxy yang akan terlihat oleh user (Sintaks: `visible_hostname 'NAMA_YANG_DIINGINKAN'`)

**STEP 4** - Restart squid dengan cara mengetikkan perintah:

```
service squid restart
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image39.png?raw=true)

**STEP 5** - Ubah pengaturan proxy browser. Gunakan **IP Ardx** sebagai host dan isikan port **8080**. Kemudian cobalah untuk mengakses web http://ittelkom-sby.ac.id (usahakan menggunakan mode **incognito/private**).

**STEP 5** - Pada Monx, buka `nano ~/.bashrc` lalu masukkan

![1700624514217](image/README/1700624514217.png)

```
export http_proxy="http://192.168.164.4:8080/"
export https_proxy="https://192.168.164.4:8080/"
export ftp_proxy="ftp://192.168.164.4:8080/"

//192.168.164.4 IP dari Ardx
//8080 port dari Ardx

```

```
//setelah selesai ketik
source ~/.bashrc

```

**STEP 6** - Coba akses `curl www.google.com` akan terjadi error (acces denied) krn tidak di allow pada proxy Ardx

![1700624757014](image/README/1700624757014.png)

**STEP 7** - Untuk lebih jelas coba akses `curl -I www.google.com`

![1700624875930](image/README/1700624875930.png)

**STEP 6** - Supaya bisa mengakses web www.google.com, maka kalian harus menambah sebaris script pada konfigurasi squid. Buka kembali file konfigurasi tadi dan tambahkan baris berikut:

```
nano /etc/squid/squid.conf
```

```
http_access allow all
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image42.png?raw=true)

**Keterangan:**

- `http_access allow all` : Memperbolehkan semuanya untuk mengakses proxy via http. Pengaturan ini perlu ditambahkan karena pengaturan default squid adalah **deny** (Sintaks: `http_access allow 'TARGET'`)
- Untuk menolak koneksi, maka **allow** diganti dengan **deny**.

**STEP 9** - **Simpan** file konfigurasi tersebut, lalu **restart** squid `service squid restart`. Refresh halaman web www.google.com.

Seharusnya halaman yang ditampilkan kembali normal.

![1700625132492](image/README/1700625132492.png)

### 2.2.3 Membuat User Login

**STEP 1** - Install `apache2-utils` pada UML **Ardx**. Sebelumnya kalian sudah harus melakukan `apt-get update`. Ketikkan:

```
apt-get install apache2-utils
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image44.png?raw=true)

**STEP 2** - Buat user dan password baru. Ketikkan:

```
htpasswd -c /etc/squid/passwd jarkom85

//username jarkom85
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image45.png?raw=true)

Ketikkan password yang diinginkan. Jika sudah maka akan muncul notifikasi:

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image46.png?raw=true)

**STEP 3** - Edit konfigurasi squid menjadi:

```
http_port 8080
visible_hostname Ardx

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image47.png?raw=true)

**Keterangan:**

- `auth_param` digunakan untuk mengatur autentikasi (Sintaks: `auth_param 'SCHEME' 'PARAMETER' 'SETTING'`. Lebih lengkapnya di http://www.squid-cache.org/Doc/config/auth_param/).
- `program` : Perintah untuk mendefiniskan autentikator eksternal.
- `children` : Mendefinisikan jumlah maksimal autentikator muncul.
- `realm` : Teks yang akan muncul pada pop-up autentikasi.
- `credentialsttl` : Mengatur masa aktif suatu autentikasi berlaku.
- `casesensitive` : Mengatur apakah **username** bersifat case sensitive atau tidak.
- `acl` digunakan untuk mendefinisikan pengaturan akses tertentu. (Sintaks umum: **acl ACL_NAME ACL_TYPE ARGUMENT** . Lebih lengkapnya di http://www.squid-cache.org/Doc/config/acl/)
- Untuk melihat daftar apa saja yang bisa diatur dengan acl bisa diakses di: https://wiki.squid-cache.org/SquidFaq/SquidAcl)

**STEP 4** - Restart squid

**STEP 5** - Coba ketik lagi `curl www.google.com`. maka akan terjadi error lg diminta untuk autentikasi
![1700626454851](image/README/1700626454851.png)

**STEP 6** - Buat script baru dengan `nano set_proxy.sh`

```
#!/bin/bash

# Minta pengguna memasukkan username dan password
read -p "Enter Proxy Username: " proxy_user
read -sp "Enter Proxy Password: " proxy_pass
echo

# Mengatur variabel lingkungan
export http_proxy="http://$proxy_user:$proxy_pass@192.168.164.4:8080"
export https_proxy="https://$proxy_user:$proxy_pass@192.168.164.4:8080"
export ftp_proxy="ftp://$proxy_user:$proxy_pass@192.168.164.4:8080"

# Memberikan konfirmasi
echo "Proxy variables set."
```

**STEP 7** - Ubah script dengan `chmod +x set_proxy.sh`

**STEP 8** - jalankan script dengan `source set_proxy.sh` setelah itu masukkan username (jarkom85) dan password

**STEP 9** - Coba ketik lagi `curl www.google.com`. sudah berhasil

![1700626999368](image/README/1700626999368.png)

### 2.2.4 Pembatasan Waktu Akses

Kita akan mencoba membatasi akses proxy pada hari dan jam tertentu. Asumsikan proxy dapat digunakan hanya pada hari **Senin** sampai **Jumat** pada jam **08.00-20.00**.

**STEP 1** - Buat file baru bernama **acl.conf** di folder **squid**

```
nano /etc/squid/acl.conf
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image49.png?raw=true)

**STEP 2** - Tambahkan baris berikut

```
acl KERJA time MTWHF 08:00-20:00
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image50.png?raw=true)

**STEP 3** - Simpan file **acl.conf**.

**STEP 4** - Buka file **squid.conf**.

```
nano /etc/squid/squid.conf
```

**STEP 5** - Ubah konfigurasinya menjadi:

```
include /etc/squid/acl.conf

http_port 8080
http_access allow KERJA
http_access deny all
visible_hostname Ardx
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image51.png?raw=true)

**STEP 6** - Simpan file tersebut. Kemudian restart squid.

**STEP 7** - Cobalah untuk mengakses `curl www.google.com`. Akan muncul halaman error jika mengakses diluar waktu yang telah ditentukan.

![1700630013558](image/README/1700630013558.png)

Keterangan:

- **MTWHF** adalah hari-hari dimana user diperbolehkan menggunakan proxy. **(S: Sunday, M: Monday, T: Tuesday, W: Wednesday, H: Thursday, F: Friday, A: Saturday)**
- Penulisan jam menggunakan format: **h1:m1-h2:m2**. Dengan syarat **h1<h2** dan **m1<m2**

### 2.2.5 Pembatasan Akses ke Website Tertentu

Kita akan mencoba membatasi akses ke beberapa website. Untuk contoh disini, kita akan memblokir website **elearning.ittelkom-sby.ac.id**

**STEP 1** - Buat file bernama **bad-sites.acl** di folder **squid** dengan mengetikkan:

```
nano /etc/squid/bad-sites.acl
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image53.png?raw=true)

**STEP 2** - Tambahkan alamat url yang akan diblock seperti baris berikut:

```
elearning.ittelkom-sby.ac.id
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image54.png?raw=true)

**STEP 3** - Ubah file konfigurasi squid menjadi seperti berikut ini.

```
http_port 8080
visible_hostname Ardx

acl BLACKLISTS dstdomain "/etc/squid/bad-sites.acl"
http_access deny BLACKLISTS
http_access allow all
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image55.png?raw=true)

**STEP 4** - Restart squid. Kemudian cobalah untuk mengakses `curl elearning.ittelkom-sby.ac.id`

![1700630720838](image/README/1700630720838.png)

Keterangan:

- **dstdomain** artinya destination domain/domain tujuan. Sintaksnya bisa diikuti dengan nama domain tujuan atau file yang menampung list-list alamat website.

### 2.2.6 Pembatasan Bandwidth

Kita akan mencoba untuk membatasi bandwidth yang akan diberikan kepada user proxy. Untuk contoh disini kita akan membatasi penggunaannya maksimal 512 kbps.

**STEP 0** - Install speedtest apt-get install speedtest-cli. kemudian run ` speedtest-cli`

**STEP 1** - Buat file bernama **acl-bandwidth.conf** di folder squid

```
nano /etc/squid/acl-bandwidth.conf
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image57.png?raw=true)

**STEP 2** - Ketikkan baris berikut

```
delay_pools 1
delay_class 1 1
delay_access 1 allow all
delay_parameters 1 16000/64000
```



1. `delay_pools 1`: Ini menentukan jumlah 'delay pools' (kelompok penundaan) yang akan digunakan. Angka '1' di sini menunjukkan bahwa hanya satu kelompok penundaan yang akan diatur. Delay pools digunakan untuk mengelola dan membatasi penggunaan bandwidth.
2. `delay_class 1 1`: Ini menetapkan kelas dari delay pool pertama. Squid memiliki tiga kelas delay pool, dengan '1' sebagai kelas paling sederhana. Kelas 1 memungkinkan Anda mengatur batas bandwidth secara keseluruhan untuk semua pengguna atau alamat IP yang dicakup oleh pool ini.
3. `delay_access 1 allow all`: Ini mendefinisikan aturan akses untuk delay pool. Dalam hal ini, 'delay_access 1' menunjukkan aturan untuk delay pool nomor 1, dan 'allow all' berarti aturan ini berlaku untuk semua pengguna atau permintaan. Ini berarti tidak ada pembatasan khusus terhadap pengguna mana yang terpengaruh oleh aturan ini.
4. `delay_parameters 1 16000/64000`: Ini adalah parameter kunci yang menetapkan batasan bandwidth. 'delay_parameters 1' menunjukkan bahwa parameter ini diterapkan pada delay pool pertama. '16000/64000' merupakan dua angka yang dipisahkan oleh garis miring. Angka pertama (16000) adalah batas bandwidth 'aggregate' (total) dalam bytes per detik untuk semua pengguna atau koneksi dalam pool ini. Angka kedua (64000) adalah batas maksimum bandwidth yang dapat digunakan oleh setiap pengguna atau koneksi individual dalam pool tersebut. Jadi, secara total, delay pool ini dapat menggunakan hingga 16 KB/detik, dengan setiap pengguna atau koneksi dibatasi maksimum 64 KB/detik.

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image58.png?raw=true)

**STEP 3** - Ubah konfigurasi squid menjadi:

```
include /etc/squid/acl-bandwidth.conf
http_port 8080
visible_hostname Ardx

http_access allow all
```

![jpeg](https://github.com/jossman14/ModulJarkomInformatikaITTS/blob/modul-3/img/image59.png?raw=true)

**STEP 4** - Restart Squid

**STEP 5** - Cobalah untuk melakukan speed test dengan ` speedtest-cli` . Berikut perbedaan sebelum dan sesudah adanya pembatasan bandwidth saat melakukan speed test

|                                                                                        Sebelum                                                                                        |                    Setelah                    |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------: |
| ![1700632029479](https://file+.vscode-resource.vscode-cdn.net/d%3A/Materi%20Random/Materi/Jaringan%20Komputer/Praktikum/ModulJarkomInformatikaITTS/Proxy/image/README/1700632029479.png) | ![1700632211672](image/README/1700632211672.png) |

Keterangan:

- **delay_pools** digunakan untuk menentukan berapa bagian/pool yang akan dibuat. (Sintaks: **delay_pools JUMLAH_YANG_DIINGINKAN**. Lebih lengkap lihat di http://www.squid-cache.org/Doc/config/delay_pools/).
- **delay_class** digunakan untuk menentukan tipe/class pembagian bandwith dari setiap pool. (Sintaks: **delay_class POOL_KE_BERAPA KELAS**.) Lebih lengkap lihat di http://www.squid-cache.org/Doc/config/delay_class/.
- **delay_access** mirip seperti http_access, tetapi digunakan untuk mengakses pool yang telah dibuat (Sintaks: **delay_access POOL_KE_BERAPA allow/deny TARGET**. Lebih lengkap lihat di http://www.squid-cache.org/Doc/config/delay_access/).
- **delay_parameters** digunakan untuk mengatur parameter dari pool yang telah dibuat. Sintaks berbeda-beda sesuai dengan tipe/kelas dari pool yang dibuat. Lebih lengkap lihat di http://www.squid-cache.org/Doc/config/delay_parameters/
- **16000/64000** **(restore/max)** **restore** menentukan besarnya bandwith dalam satuan bytes/second **max** menentukan besarnya file atau bucket yang dapat dilewatkan tanpa melalui delay dalam satuan bytes.
- Penjelasan dari fitur **delay_pools** lebih lengkap bisa dilihat di https://wiki.squid-cache.org/Features/DelayPools
