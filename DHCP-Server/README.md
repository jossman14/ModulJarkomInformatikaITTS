# 1. Dynamic Host Configuration Protocol (DHCP)

## 1.1 Konsep

### 1.1.1 Pendahuluan

Pada modul sebelumnya, kita telah mempelajari cara mengonfigurasi alamat IP, nameserver, gateway, dan subnetmask pada UML. Metode konfigurasi manual ini dapat diimplementasikan dengan mudah pada jaringan yang hanya memiliki sedikit host. Namun, apa yang terjadi jika suatu jaringan memiliki banyak host? Apakah Administrator Jaringan harus melakukan semua konfigurasi tersebut secara manual satu persatu ke semua host? Tentu tidak kawan!

Misalnya pada kasus jaringan WiFi umum: Apakah kalian pernah terfikir, siapakah yang bertugas memberikan alamat IP pada setiap client yang terhubung ke jaringan WiFi? Tentu saja pengalokasian dan pemberian alamat IP tersebut tidak dilakukan secara manual karena client yang terhubung sangatlah banyak dan berganti-ganti.

Adapun kasus ini dapat diselesaikan oleh DHCP.

### 1.1.2 Apa itu DHCP?

**DHCP (Dynamic Host Configuration Protocol)** adalah protokol yang berbasis arsitektur client/server yang dipakai untuk memudahkan pengalokasian alamat IP dalam satu jaringan. DHCP secara otomatis meminjamkan alamat IP kepada host yang memintanya.

![image4.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image4.png?raw=true)

Tanpa DHCP, administrator jaringan harus memasukkan alamat IP masing-masing komputer dalam suatu jaringan secara manual. Namun jika DHCP dipasang di jaringan, maka semua komputer yang tersambung ke jaringan akan mendapatkan alamat IP secara otomatis dari DHCP server.

### 1.1.3 Bootstrap Protocol dan Dynamic Host Configuration Protocol

Selain DHCP, terdapat protokol lain yang juga memudahkan pengalokasian alamat IP dalam suatu jaringan, yaitu Bootstrap Protocol (BOOTP). Perbedaan BOOTP dan DHCP terletak pada proses konfigurasinya.

Perbedaan BOOTP dan DHCP terletak pada proses konfigurasinya.

|                                            BOOTP                                            |                                                                        DHCP                                                                        |
| :-----------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------: |
| Administrator jaringan melakukan konfigurasi mapping MAC Address client dengan IP tertentu. | Server akan melakukan peminjaman IP Address dan konfigurasi lainnya dalam rentang waktu tertentu. Protokol ini dibuat berdasarkan cara kerja BOOTP. |

### 1.1.4 DHCP Message Header

![image5.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image5.png?raw=true)

Keterangan:

![image6.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image6.png?raw=true)

### 1.1.5 Cara Kerja DHCP

DHCP bekerja dengan melibatkan dua pihak yakni **Server** dan **Client**:

1. **DHCP Server** memberikan suatu layanan yang dapat memberikan alamat IP dan parameter lainnya kepada semua client yang memintanya.
2. **DHCP Client** adalah mesin client yang menjalankan perangkat lunak client yang memungkinkan mereka untuk dapat berkomunikasi dengan DHCP server.

DHCP Server umumnya memiliki sekumpulan alamat IP yang didistribusikan yang disebut **DHCP Pool**. Setiap client akan meminjamnya untuk rentan waktu yang ditentukan oleh DHCP sendiri (dalam konfigurasi). Jika masa waktu habis, maka client akan meminta alamat IP yang baru atau memperpanjangnya. Itulah sebabnya alamat IP client menjadi dinamis.

![image7.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image7.gif?raw=true)

Terdapat 4 tahapan yang dilakukan dalam proses peminjaman alamat IP pada DHCP:

1. **DHCPDISCOVER**: Client menyebarkan request secara broadcast untuk mencari DHCP Server yang aktif. DHCP Server menggunakan UDP port 67 untuk menerima broadcast dari client melalui port 68.
2. **DHCPOFFER**: DHCP server menawarkan alamat IP (dan konfigurasi lainnya apabila ada) kepada client. Alamat IP yang ditawarkan adalah salah satu alamat yang tersedia dalam DHCP Pool pada DHCP Server yang bersangkutan.
3. **DHCPREQUEST**: Client menerima tawaran dan menyetujui peminjaman alamat IP tersebut kepada DHCP Server.
4. **DHCPACK**: DHCP server menyetujui permintaan alamat IP dari client dengan mengirimkan paket ACKnoledgment berupa konfirmasi alamat IP dan informasi lain. Kemudian client melakukan inisialisasi dengan mengikat (binding) alamat IP tersebut dan client dapat bekerja pada jaringan tersebut. DHCP Server akan mencatat peminjaman yang terjadi.
5. **DHCPRELEASE**: Client menghentikan peminjaman alamat IP (apabila waktu peminjaman habis atau menerima DHCPNAK).

![image8.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image8.png?raw=true)

 Lebih lanjut: https://www.nada.kth.se/kurser/kth/2D1392/05/lectures/lecture_9.pdf

## 1.2 Implementasi

### 1.2.1 Instalasi ISC-DHCP-Server

Pada topologi ini, kita akan menjadikan router **Dzul** sebagai DHCP Server. Oleh karena itu, kita harus menginstal **isc-dhcp-server** di **Dzul** dengan melakukan langkah-langkah berikut:

1. Update package lists di router **Dzul**

   ```
   apt-get update
   ```
2. Install **isc-dhcp-server** di router **Dzul**

   ```
   apt-get install isc-dhcp-server -y
   ```

   ![image9.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image9.png?raw=true)

   **[FAIL]** Eits, jangan panik dulu!!! Coba dibaca baik-baik, yang gagal bukanlah proses instalasinya, namun proses `starting ISC DHCP server`. Hal ini terjadi karena kita belum mengonfigurasi interface-nya. Mari kita lanjutkan ke langkah selanjutnya!

### 1.2.2 Konfigurasi DHCP Server

Langkah pertama yang harus dilakukan setelah instalasi adalah **menentukan interface** mana yang akan diberikan layanan DHCP. Konfigurasi interface terletak di `/etc/default/isc-dhcp-server`.

Maka, lakukanlah:

1. Membuka file konfigurasi

   ```
   nano /etc/default/isc-dhcp-server
   ```
2. Menentukan interface. Coba cermati topologi kalian. Interface dari router **Dzul** yang menuju ke client **Walx**, **Monx**, dan **Zonx** adalah `eth2`, maka kita akan memilih interface `eth2` untuk diberikan layanan DHCP.

   ```
   INTERFACESv4="eth2"
   ```

   ![image10.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image10.png?raw=true)

Setelah menentukan interface, langkah selanjutnya adalah **mengonfigurasi DHCP**-nya. Ada banyak hal yang dapat dikonfigurasi, antara lain:

- Range IP
- DNS Server
- Informasi Netmask
- Default Gateway
- dll

Konfigurasi DHCP terletak di `/etc/dhcp/dhcpd.conf`. Langkah-langkah yang harus dilakukan adalah:

1. Buka file konfigurasi

   ```
   nano /etc/dhcp/dhcpd.conf
   ```
2. Tambahkan script berikut:

   ```
   subnet 'IP_Address_Eth2' netmask 'Netmask' {
       range 'IP_Awal' 'IP_Akhir';
       option routers 'iP_Gateway';
       option broadcast-address 'IP_Broadcast';
       option domain-name-servers 'IP_Aldx';
       default-lease-time 'Waktu';
       max-lease-time 'Waktu';
   }
   ```

   Script tersebut mengatur parameter jaringan yang dapat didistribusikan oleh DHCP, seperti informasi netmask, default gateway dan DNS server. Berikut ini beberapa **parameter jaringan dasar** yang biasanya digunakan:

   | No | Parameter Jaringan                          | Keterangan                                                                                                                                                                                                                                                                                              |
   | -- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | 1  | `subnet 'IP_Address_Eth2'`                | Network ID pada subnet. Default (192.168.0.0)                                                                                                                                                                                                                                                           |
   | 2  | `netmask 'Netmask'`                       | Netmask pada subnet.                                                                                                                                                                                                                                                                                    |
   | 3  | `range 'IP_Awal' 'IP_Akhir'`              | Rentang alamat IP yang akan didistribusikan dan digunakan secara dinamis.                                                                                                                                                                                                                               |
   | 4  | `option routers 'Gateway'`                | IP gateway dari router menuju client sesuai konfigurasi subnet.                                                                                                                                                                                                                                         |
   | 5  | `option broadcast-address 'IP_Broadcast'` | IP broadcast pada subnet.                                                                                                                                                                                                                                                                               |
   | 6  | `option domain-name-servers 'IP_Aldx'`    | DNS yang ingin kita berikan pada client. Gunakan IP Aldx                                                                                                                                                                                                                                                |
   | 7  | Lease time                                  | Waktu yang dialokasikan ketika sebuah IP dipinjamkan kepada komputer client. Setelah waktu pinjam ini selesai, maka IP tersebut dapat dipinjam lagi oleh komputer yang sama atau komputer tersebut mendapatkan alamat IP lain jika alamat IP yang sebelumnya dipinjam, dipergunakan oleh komputer lain. |
   | 8  | `default-lease-time 'Waktu'`              | Lama waktu DHCP server meminjamkan alamat IP kepada client, dalam satuan detik. Default 600 detik.                                                                                                                                                                                                      |
   | 9  | `max-lease-time 'Waktu'`                  | Waktu maksimal yang di alokasikan untuk peminjaman IP oleh DHCP server ke client dalam satuan detik. Default 7200 detik.                                                                                                                                                                                |

   ![image11.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image11.png?raw=true)

   **Cara Mendapatkan range IP Awal dan IP Akhir**

   Untuk range IP address di ambil dari **NID DMZ** baris terakhir pada setiap kelompok

   Sebagai contoh :

   IP range akan kita gunakan yaitu 192.168.0.** (Kita Akan mencari IP Terakhir)**

   Jarkom A Memiliki NID DMZ 192.168.164.**1** maka yang di ambil IP pada baris terakhir **1** sebagai **IP range Awal**.

   Untuk pendapatkan **IP range Akhir**, tinggal nambahkan saja **IP range Awal yang didapat + 7** maka akan akan mendapatkan **8.**

   Maka didapatkan

   \-     **IP range Awal : 192.168.0.1**

   \-     **IP range Akhir : 192.168.0.8**

   Sehingga, konfigurasinya menjadi seperti ini:


   ```
   subnet 192.168.0.0 netmask 255.255.255.0 {

     range 192.168.0.1 192.168.0.8;

     option routers 192.168.0.1;

     option broadcast-address 192.168.0.255;

     option domain-name-servers 192.168.164.5;

     default-lease-time 600;

     max-lease-time 7200;

   }
   ```

   ![image13.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image13.png?raw=true)
3. Jangan lupa restart servicenya!

   ```
   service isc-dhcp-server restart
   ```

   Jika terjadi **failed!**, maka stop dulu, kemudian start kembali

   ```
   service isc-dhcp-server stop
   service isc-dhcp-server start
   ```

![image14.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image14.png?raw=true)

Konfigurasi DHCP Server selesai!

### 1.2.3 Konfigurasi DHCP Client

Setelah mengonfigurasi server, kita juga perlu mengonfigurasi interface client supaya client bisa mendapatkan layanan dari DHCP server. Di dalam topologi ini, clientnya adalah **Walx**, **Monx**, dan **Zonx**.

1. Sebelumnya, coba cek terlebih dahulu IP **Walx** dengan `ifconfig`

   ![image15.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image15.png?raw=true)

   Dari konfigurasi sebelumnya, **Walx** telah diberikan IP static **192.168.0.2**
2. Kemudian buka `/etc/network/interfaces` untuk mengonfigurasi interface **Walx**.

   ```
   nano /etc/network/interfaces
   ```
3. Comment atau hapus konfigurasi yang lama, kemudian tambahkan script ini.

   ```
   auto eth0
   iface eth0 inet dhcp
   ```

   ![image16.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image16.png?raw=true)

   Keterangan:

   - **eth0** adalah interface yang digunakan client
   - `iface eth0 inet dhcp` : interface eth0 diberikan konfigurasi DHCP, bukan static
4. Jangan lupa restart!

   ```
   service networking restart
   ```

   ![image17.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image17.png?raw=true)
5. Testing

   Coba cek kembali IP **Walx** dengan melakukan `ifconfig`

   ![image18.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image18.png?raw=true)

   Cek pula apakah **Walx** sudah mendapatkan DNS server sesuai konfigurasi di DHCP. Cek di `/etc/resolv.conf`, dengan menggunakan command:

   ```
   cat /etc/resolv.conf
   ```

   ![image19.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image19.png?raw=true)

   Yeay! IP **Walx** telah berubah sesuai dengan range IP yang diberikan oleh DHCP Server, serta nameserver-nya otomatis terkonfigurasi mengarah ke **10.151.51.83(IP Aldx)**. DHCP kalian berhasil!

   **Keterangan** :

   - Jika IP **Walx** masih belum berubah, jangan panik. Lakukanlah kembali `service networking restart`
   - Jika masih belum berubah juga, jangan buru-buru bertanya. Coba cek lagi semua konfigurasi yang telah kamu lakukan, mungkin ada typo-typo.

Lakukan kembali langkah-langkah di atas pada client **Monx** dan **Zonx**.

- Client **Monx**

  ![image20.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image20.png?raw=true)

  ![image21.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image21.png?raw=true)
- Client **Zonx**

  ![image22.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image22.png?raw=true)

  ![image23.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image23.png?raw=true)

Setelah IP dipinjamkan ke sebuah client, maka IP tersebut tidak akan diberikan ke client lain. Buktinya, tidak ada client yang mendapatkan IP yang sama.

### 1.2.4 Fixed Address

> **Sebuah Kasus:**
>
> Ternyata PC **Zonx** selain menjadi client, juga akan digunakan sebagai server suatu aplikasi, sehingga akan menyulitkan jika IP nya berganti-ganti setiap **Zonx** terhubung ke jaringan internet. Oleh karena itu, **Zonx** membutuhkan IP yang tidak berganti-ganti.

Untuk menyelesaikan kasus tersebut, DHCP Server memiliki layanan untuk "menyewakan" alamat IP secara tetap pada suatu host, yakni **Fixed Address**. Dalam kasus ini, **Zonx** akan mendapatkan IP tetap ***192.168.0.85**.*

Lakukanlah:

1. Buka file konfigurasi DHCP Server di router **Dzul**

   ```
   nano /etc/dhcp/dhcpd.conf
   ```

   Dan tambahkan script berikut:

   ```
   host Zonx {
       hardware ethernet 'hwaddress_Zonx';
       fixed-address 192.168.0.6;
   }
   ```

   ![image24.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image24.png?raw=true)

   **Penjelasan:**

   - Untuk mencari `'hwaddress_Zonx'` (hardware address) kalian bisa mengeceknya di UML **Zonx** dengan command `ifconfig`

     ![image25.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image25.png?raw=true)
   - **fixed-address** adalah alamat IP yang "disewa" tetap oleh **Zonx**
2. Jangan lupa restart!

   ```
   service isc-dhcp-server restart
   ```

Setelah mengonfigurasi DHCP server, kita juga harus mengonfigurasi DHCP client. Lakukanlah:

1. Buka file konfigurasi interface **Zonx**

   ```
   nano /etc/network/interfaces
   ```
2. Tambahkan script berikut pada konfigurasi

   ```
   hwaddress ether 'hwaddress_Zonx'
   ```

   ![image26.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image26.png?raw=true)

   **Keterangan:** Hardware address perlu di-setting juga di **/etc/network/interfaces** karena perangkat yang kalian gunakan adalah perangkat virtual (UML) dimana hwaddress-nya akan berubah setiap kali dijalankan.
3. Jangan lupa restart!

   ```
   service networking restart
   ```

   ![image27.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image27.png?raw=true)
4. Testing

   Coba cek IP **Zonx** dengan melakukan `ifconfig`

   ![image28.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image28.png?raw=true)

   Yeay! **IP Zonx**  telah berubah menjadi **192.168.0.85** sesuai dengan Fixed Address yang diberikan oleh DHCP Server.

### 1.2.5 Testing

Setelah melakukan berbagai konfigurasi di atas, kalian bisa memastikan apakah DHCP Server kalian berhasil dengan cara:

1. Matikan UML kalian

   ```
   bash bye.sh
   ```
2. Jalankan UML kembali

   ```
   bash topologi.sh
   ```
3. Cek IP di semua client dengan `ifconfig`.

   ![image29.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image29.png?raw=true)

   ![image30.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image30.png?raw=true)

   ![image31.png](https://github.com/aldonesia/ModulJarkomInformatikaITTS/blob/modul-3/img/image31.png?raw=true)

Jika **Walx** dan **Monx** berganti alamat IP sesuai dengan range yang telah dikonfigurasi DHCP dan **Zonx** tetap mendapatkan **IP 192.168.0.85**, maka konfigurasi DHCP server kalian berhasil.

## 1.3 Soal Latihan

1. Buatlah konfigurasi DHCP agar **Walx** dan **Monx** mendapatkan IP dengan range *192.168.0.100 - 192.168.0.110* dan *192.168.0.112 - 192.168.0.116* dengan syarat:
   - Setiap 1 menit IP yang digunakan client berganti
   - DNS mengarah ke DNS server kalian sendiri yaitu **Aldx**, tetapi tetap bisa digunakan untuk mengakses internet
2. Bagaimana cara konfigurasi DHCP dengan 2 interface?
