# **PENTING UNTUK DIBACA**
1. Pastikan UML *Ardx* ,*Aldx* Memory yang sebelumnya **128 diganti 256** pada file topologi.sh. Sedangkan untuk UML *Walx dan Monx* diganti menjadi *128 dari yang awalnya 64*

2. Jalankan **iptables** agar client *Walx* dan *Monx* bisa terhubung ke internet.
```shell
iptables-legacy -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s NID_DMZ
iptables-legacy -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.168.0.0/16
```

# 1. DNS (Domain Name System)

## 1.1 Teori

### 1.1.A Pengertian

DNS (_Domain Name System_) adalah sistem penamaan untuk semua device (smartphone, computer, atau
network) yang terhubung dengan internet. DNS Server berfungsi menerjemahkan nama domain menjadi alamat IP. DNS dibuat guna untuk menggantikan sistem penggunaan file host yang dirasa tidak efisien.

### 1.1.B Cara Kerja

<img src="https://i.ibb.co/L8DBRpJ/1.jpg" alt="1" border="0" />

Client akan meminta alamt IP dari suatu domain ke DNS server. Jika pada DNS server data alamat IP dari DNS server tersebut ada maka akan di return alamat IP nya kembali menuju client. Jika DNS server tersebut tidak memiliki alamat IP dari domain tersebut maka dia akan bertanya kepada DNS server yang lain sampai alamat domain itu ditemukan.

### 1.1.C Aplikasi DNS Server

Untuk praktikum jarkom kita menggunakan aplikasi BIND9 sebagai DNS server, karena BIND(Berkley Internet Naming Daemon) adalah DNS server yang paling banyak digunakan dan juga memiliki fitur-fitur yang cukup lengkap.

### 1.1.D List DNS Record
| Tipe          | Deskripsi                     |
| ------------- |:-----------------------------|
| A             | Memetakan nama domain ke alamat IP (IPv4) dari komputer hosting domain|
| AAAA          | AAAA record hampir mirip A record, tapi mengarahkan domain ke alamat Ipv6|
| CNAME         | Alias ​​dari satu nama ke nama lain: pencarian DNS akan dilanjutkan dengan mencoba lagi pencarian dengan nama baru|
| NS            | Delegasikan zona DNS untuk menggunakan authoritative name servers yang diberikan|
| PTR           | Digunakan untuk Reverse DNS (Domain Name System) lookup|
| SOA           | Mengacu server DNS yang mengediakan otorisasi informasi tentang sebuah domain Internet|
| TXT           | Mengijinkan administrator untuk memasukan data acak ke dalam catatan DNS, catatan ini juga digunakan di spesifikasi Sender Policy Framework|

### 1.1.E SOA (Start of Authority)

Adalah informasi yang dimiliki oleh suatu DNS zone.

| Nama          | Deskripsi                     |
| ------------- |:-----------------------------|
| Serial        | Jumlah revisi dari file zona ini. Kenaikan nomor ini setiap kali file zone diubah sehingga perubahannya akan didistribusikan ke server DNS sekunder manapun|
| Refresh       | Jumlah waktu dalam detik bahwa nameserver sekunder harus menunggu untuk memeriksa salinan baru dari zona DNS dari nameserver utama domain. Jika file zona telah berubah maka server DNS sekunder akan memperbarui salinan zona tersebut agar sesuai dengan zona server DNS utama|
| Retry         | Jumlah waktu dalam hitungan detik bahwa nameserver utama domain (atau server) harus menunggu jika upaya refresh oleh nameserver sekunder gagal sebelum mencoba refresh zona domain dengan nameserver sekunder itu lagi|
| Expire        | Jumlah waktu dalam hitungan detik bahwa nameserver sekunder (atau server) akan menahan zona sebelum tidak lagi mempunyai otoritas|
| Minimum       | Jumlah waktu dalam hitungan detik bahwa catatan sumber daya domain valid. Ini juga dikenal sebagai TTL minimum, dan dapat diganti oleh TTL catatan sumber daya individu|
| TTL           | (waktu untuk tinggal) - Jumlah detik nama domain di-cache secara lokal sebelum kadaluarsa dan kembali ke nameserver otoritatif untuk informasi terbaru|



------





## 1.2 Praktik

### 1.2.A Buat Topologi Berikut

<img src="https://i.ibb.co/0VsQ35c/Screenshot-15.png" alt="Screenshot-15" border="0">

Referensi 

[Modul Pengenalan UML](https://github.com/rohanaq/Modul-Pengenalan-UML "Modul Pengenalan UML")

### 1.2.A Instalasi bind

- Buka *Ardx* dan update package lists dengan menjalankan command:

	```
	apt-get update
	```

  <img src="https://i.ibb.co/L5hPFbQ/1.png" alt="1" border="0">

- Setalah melakukan update silahkan install aplikasi bind9 pada *Ardx* dengan perintah:

	```
	apt-get install bind9 -y
	```

  <img src="https://i.ibb.co/CQBq1mc/2.png" alt="2" border="0">

### 1.2.B Pembuatan Domain
Pada sesilab ini kita akan membuat domain **jarkomittsts.com**.

- Lakukan perintah pada *Ardx*. Isikan seperti berikut:

  ```
   nano /etc/bind/named.conf.local
  ```

- Isikan configurasi domain **jarkomitts.com** sesuai dengan syntax berikut:

  ```
  zone "jarkomitts.com" {
  	type master;
  	file "/etc/bind/jarkom/jarkomitts.com";
  };
  ```

  <img src="https://i.ibb.co/2FZ8Brd/3.png" alt="3" border="0">

- Buat folder **jarkom** di dalam **/etc/bind**

  ```
  mkdir /etc/bind/jarkom
  ```
  <img src="https://i.ibb.co/xX0mxJs/4.png" alt="4" border="0">

- Copykan file **db.local** pada path **/etc/bind** ke dalam folder **jarkom** yang baru saja dibuat dan ubah namanya menjadi **jarkomitts.com**

  ```shell
  cp /etc/bind/db.local /etc/bind/jarkom/jarkomitts.com
  ```

  <img src="https://i.ibb.co/C1p1Ybj/image.png" alt="image" border="0">

- Kemudian buka file **jarkomitts.com** dan edit seperti gambar berikut dengan IP *Ardx* masing-masing kelompok:

  ```shell
  nano /etc/bind/jarkom/jarkomitts.com
  ```
  <img src="https://i.ibb.co/W5KVd7g/image.png" alt="image" border="0">

- Restart bind9 dengan perintah 

  ```shell
  service named restart
  ```
  <img src="https://i.ibb.co/sH9fQZG/image.png" alt="image" border="0">

### 1.2.C Setting nameserver pada client

Domain yang kita buat tidak akan langsung dikenali oleh client oleh sebab itu kita harus merubah settingan nameserver yang ada pada client kita.

- Pada client *Walx* dan *Monx* arahkan nameserver menuju IP *Ardx* dengan mengedit file _resolv.conf_ dengan mengetikkan perintah 

	```shell
	nano /etc/resolv.conf
	```

  <img src="https://i.ibb.co/XDd0X5G/image.png" alt="image" border="0">

- Untuk mencoba koneksi DNS, lakukan ping domain **jarkomitts.com** dengan melakukan  perintah berikut pada client *Walx* dan *Monx*

  ```
  ping jarkomitts.com
  ```

  <img src="https://i.ibb.co/fSsdZ7R/image.png" alt="image" border="0">



### 1.2.D Reverse DNS (Record PTR)

Jika pada pembuatan domain sebelumnya DNS server kita bekerja menerjemahkan string domain **jarkomitts.com** kedalam alamat IP agar dapat dibuka, maka Reverse DNS atau Record PTR digunakan untuk menerjemahkan alamat IP ke alamat domain yang sudah diterjemahkan sebelumnya.

- Edit file **/etc/bind/named.conf.local** pada *Ardx*

  ```shell
  nano /etc/bind/named.conf.local
  ```

- Lalu tambahkan konfigurasi berikut ke dalam file **named.conf.local**

  ```shell
  zone "151.151.10.in-addr.arpa" {
      type master;
      file "/etc/bind/jarkom/151.151.10.in-addr.arpa";
  };
  ```
  *Keterangan 151.151.10 adalah 3 byte pertama IP Ardx yang dibalik urutan penulisannya*

  <img src="https://i.ibb.co/qjR2WYV/image.png" alt="image" border="0">


- Copykan file **db.local** pada path **/etc/bind** ke dalam folder **jarkom** yang baru saja dibuat dan ubah namanya menjadi **83.151.10.in-addr.arpa**

  ```
  cp /etc/bind/db.local /etc/bind/jarkom/151.151.10.in-addr.arpa
  ```

  <img src="https://i.ibb.co/CwD28sF/image.png" alt="image" border="0">

- Edit file **151.151.10.in-addr.arpa** menjadi seperti gambar di bawah ini

  <img src="https://i.ibb.co/RgVZDWp/image.png" alt="image" border="0">

- Kemudian restart bind9 dengan perintah 

  ```
  service named restart
  ```

- Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client *Monx* 

  ```
  // Install package dnsutils
  // Pastikan nameserver telah dikembalikan ke settingan awal
  apt-get update
  apt-get install dnsutils
  //jika gagal silahkan run perintah 
  apt-get install dnsutils --fix-missing
  
  //Kembalikan nameserver agar tersambung dengan Ardx
  host -t PTR "IP Ardx"
  ```

  <img src="https://i.ibb.co/7WgpwdZ/image.png" alt="image" border="0">



### 1.2.E Record CNAME
Record CNAME adalah sebuah record yang membuat alias name dan mengarahkan domain ke alamat/domain yang lain.

Langkah-langkah membuat record CNAME:

- Buka file **jarkomitts.com** pada server *Ardx* dan tambahkan konfigurasi seperti pada gambar berikut:

  <img src="https://i.ibb.co/7jqwqsY/image.png" alt="image" border="0">

- Kemudian restart bind9 dengan perintah

  ```shell
  service named restart
  ```

- Lalu cek di Walx dengan melakukan **host -t CNAME www.jarkomitts.com** atau **ping www.jarkomitts.com**. Hasilnya harus mengarah ke host dengan IP *Ardx*.

  <img src="https://i.ibb.co/bb1CPMp/image.png" alt="image" border="0">


### 1.2.G Membuat Subdomain

Subdomain adalah bagian dari sebuah nama domain induk. Subdomain umumnya mengacu ke suatu alamat fisik di sebuah situs contohnya: **jarkomitts.com** merupakan sebuah domain induk. Sedangkan **halo.jarkomitts.com** merupakan sebuah subdomain.

- Edit file **/etc/bind/jarkom/jarkomitts.com** lalu tambahkan subdomain untuk **jarkomitts.com** yang mengarah ke IP *Ardx*.

  ```
  nano /etc/bind/jarkom/jarkomitts.com
  ```

- Tambahkan konfigurasi seperti pada gambar ke dalam file **jarkomitts.com**.

  <img src="https://i.ibb.co/bKygXdf/image.png" alt="image" border="0">

- Restart service bind  

  ```
  service named restart
  ```

- Coba ping ke subdomain dengan perintah berikut dari client *Walx*

  ```
  ping halo.jarkomitts.com
  
  ATAU
  
  host -t A halo.jarkomitts.com
  ```

  <img src="https://i.ibb.co/Qks7mm6/image.png" alt="image" border="0">


### 1.3 Keterangan Configurasi Zone file

1. #### Penulisan Serial

   Ditulis dengan format YYYYMMDDXX. Serial di increment setiap melakukan perubahan pada file zone.

   ```
   YYYY adalah tahun
   MM adalah bulan
   DD adalah tanggal
   XX adalah counter
   ```

   Contoh:

   <img src="https://i.ibb.co/Zdhz8q0/image.png" alt="image" border="0">

2. #### Penggunaan Titik

   <img src="https://i.ibb.co/xfw3Fcc/image.png" alt="image" border="0">

   Pada salah satu contoh di atas, dapat kita amati pada kolom keempat terdapat record yang menggunakan titik pada akhir kata dan ada yang tidak. Penggunaan titik berfungsi sebagai penentu FQDN (Fully-Qualified Domain Name) suatu domain.

   Contohnya jika "**jarkomitts.com.**" di akhiri dengan titik maka akan dianggap sebagai FQDN dan akan dibaca sebagai "**jarkomitts.com**" , sedangkan ns1 di atas tidak menggunakan titik sehingga dia tidak terbaca sebagai FQDN. Maka ns1 akan di tambahkan di depan terhadap nilai $ORIGIN sehinga ns1 akan terbaca sebagai "**ns1.jarkomitts.com**" . Nilai $ORIGIN diambil dari penamaan zone yang terdapat pada  */etc/bind/named.conf.local*.

3. #### Penulisan Name Server (NS) record

   Salah satu aturan penulisan NS record adalah dia harus menuju A record., bukan CNAME. 



## Latihan

1. Buatlah domain **halogaes.id** dan **www.halogaes.id** (CNAME **halogaes.id**). Apa yang terjadi jika melakukan ping **halogaes.id** dengan ping **www.halogaes.id**? Mengapa hal itu terjadi?
2. Buatlah sebuah subdomain pada domain **halogaes.id** dengan nama **annyeong.halogaes.id**!

## References
* https://computer.howstuffworks.com/dns.htm
* http://knowledgelayer.softlayer.com/faq/what-does-serial-refresh-retry-expire-minimum-and-ttl-mean
* https://en.wikipedia.org/wiki/List_of_DNS_record_types
* https://kb.indowebsite.id/knowledge-base/pengertian-catatan-dns-atau-record-dns/
