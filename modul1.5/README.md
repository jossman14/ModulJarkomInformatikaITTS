## Modul 1.5 Pengenalan UML

### Teori Dasar

UML atau User Mode Linux (UML) merupakan suatu mesin virtual yang dapat digunakan untuk menjalankan proses

pada Linux secara aman tanpa beresiko merusak lingkungan Linux yang sebenarnya. UML secara umum

bisa disebut sebagai kernel dalam kernel. Kernel dalam UML benar-benar mirip dengan kernel yang

menjalankan sebuah sistem operasi linux. Bisa diubah kodenya, dicompile,   distart, direstart, dan

dishutdown.

---

### To Do

- Download **VirtualBox** dan **Install** Silahkan mendownload dari link berikut -> https://www.virtualbox.org/wiki/Downloads
- Download **Image VirtualBox dan Impor** Silahkan mendownload dari link berikut -> [https://drive.google.com/file/u/0/d/1PTvszN3Elvrv46iaOloY6NrE-lTPpxbH/view?usp=sharing&amp;pli=1](https://drive.google.com/file/u/0/d/1PTvszN3Elvrv46iaOloY6NrE-lTPpxbH/view?usp=sharing&pli=1)
  (3GB+)

---

### Membuat Topologi yang akan Digunakan

<img src="assets\topologi.png" alt="topologi" style="zoom: 67%;" />

1. Setelah login, buat file script dengan ekstensi **.sh** yang akan digunakan untuk menyimpan script membuat **router, switch,** dan **klien**. Misalkan kita membuat file bernama **topologi.sh**.
2. Ketikkan `nano topologi.sh`
3. Sintaks yang digunakan adalah sebagai berikut:

   ```pascal
   Membuat Switch
   uml_switch –unix NAMASWITCH > /dev/null < /dev/null &

   Membuat Router & klien
   xterm –T NAMADEVICE –e linux ubd0=NAMADEVICE,jarkom umid=NAMADEVICE eth0=daemon,,,NAMASWITCH mem=96M &
   ```

   ###### Keterangan :


   - Sintaks untuk membuat router dan klien hampir sama, yang membedakan adalah jumlah eth-nya, eth pada router biasanya lebih dari 1.
   - File **root_fs** adalah iso UML yang digunakan.
   - Pembuatan jumlah router, switch, klien, dan banyaknya eth disesuaikan dengan topologi yang diminta.
4. Perintah **uml_switch -unix Vessa > /dev/null < /dev/null** dalam User-Mode Linux (UML) memiliki
   beberapa fungsi utama.

   a. **uml_switch** merupakan bagian dari UML yang bertindak sebagai switch jaringan virtual. Ini
   memungkinkan instance UML yang berbeda untuk berkomunikasi satu sama lain seperti jika mereka terhubung oleh switch jaringan fisik.

   b. Opsi **-unix Vessa** menentukan bahwa switch akan membuat dan menggunakan socket Unix dengan nama **Vessa** untuk komunikasi.

   c.Bagian **> /dev/null** mengarahkan output standar dari perintah ke  **/dev/null**, yang efektif mengabaikan output apa pun. Sementara itu, **< /dev/null** mengarahkan input standar dari perintah ke  **/dev/null** , yang berarti perintah tidak akan menerima input apa pun.
5. Perintah **xterm -T Dzul -e linux ubd0=Dzul,root_fs umid=Dzul eth0=tuntap,tap0 eth1=daemon,,,Alqis
   eth2=daemon,,,Vessa mem=128M** merupakan kombinasi dari beberapa instruksi
   untuk menjalankan emulator Linux dengan konfigurasi khusus di dalam terminal X
   (xterm). Perintah ini melakukan beberapa tugas:

   a. **xterm -T Dzul** membuka terminal X baru dengan judul 'Dzul'.
   b. **-e** menginstruksikan xterm untuk menjalankan perintah yang diberikan setelahnya.
   c. **linux** adalah emulator Linux yang dimulai di xterm.
   d. **ubd0=Dzul,root_fs** menentukan file sistem root yang digunakan oleh emulator (dalam hal ini, 'Dzul,root_fs').
   e. **umid=Dzul** menetapkan ID unik untuk instansi emulator yang berjalan.
   f. Konfigurasi jaringan dimulai dengan  **eth0=tuntap,tap0** , yang mengatur antarmuka Ethernet pertama untuk menggunakan TUN/TAP dengan nama 'tap0', dan **eth1=daemon,,,Alqis** serta **eth2=daemon,,,Vessa **menentukan antarmuka Ethernet tambahan dengan konfigurasi tertentu.
   g. **mem=128M** mengalokasikan 128 MB memori untuk emulator.
6. Untuk topologi sesuai yang ada pada gambar, maka sintaks untuk file **topologi.sh** adalah sebagai berikut:

```pascal
# Switch
uml_switch -unix Vessa > /dev/null < /dev/null &
uml_switch -unix Alqis > /dev/null < /dev/null &

# Router
xterm -T Dzul -e linux ubd0=Dzul,root_fs umid=Dzul eth0=tuntap,tap0 eth1=daemon,,,Alqis eth2=daemon,,,Vessa mem=128M &

# DNS + Web Server
xterm -T Ardx -e linux ubd0=Ardx,root_fs umid=Ardx eth0=daemon,,,Alqis mem=128M &
xterm -T Aldx -e linux ubd0=Aldx,root_fs umid=Aldx eth0=daemon,,,Alqis mem=128M &

# Klien
xterm -T Walx -e linux ubd0=Walx,root_fs umid=Walx eth0=daemon,,,Vessa mem=64M &
xterm -T Monx -e linux ubd0=Monx,root_fs umid=Monx eth0=daemon,,,Vessa mem=64M &

```

5. Jalankan script **topologi.sh** dengan perintah `bash topologi.sh`

<img src="assets\BG6.png" alt="BG6" style="zoom:67%;" />

5. Setelah muncul gambar seperti di atas, login pada masing-masing UML dengan menggunakan **Username =root** dan **tanpa password**
6. pada setiap topologi harus menggantikan nama dari **hostname** dengan mengetikan `nano /etc/hostname` sesuai dengan nama topologi

<img src="assets\BG7.png" alt="BG7" style="zoom:67%;" />

5. setelah itu mengganti nama **hostname** restart system dengan menjalankan perintah `reboot`
6. Pada router **Dzul** lakukan setting sysctl dengan mengetikkan perintah `nano /etc/sysctl.conf`
7. Hilangkan tanda pagar (#) pada bagian `net.ipv4.ip_forward=1`

   <img src="assets\BG8.png" alt="BG8" style="zoom:67%;" />
8. Lalu jalankan perintah `sysctl -p` untuk mengaktifkan perubahan yang ada. Dengan mengaktifkan fungsi ***IP Forward*** ini maka Linux nantinya dapat menentukan jalur mana yang dipilih untuk mencapai jaringan tujuan
9. Setting IP pada setiap UML dengan mengetikkan `nano /etc/network/interfaces` Lalu setting IPnya sebagai berikut:

#### NID Tuntap dan DMZ

```pascal
NID_tuntap_tiap_kelompok :  192.168.100.1
NID_DMZ_tiap_kelompok    : 192.168.(150 + no urut absen).1 -> contoh no urut absen 1 -> 192.168.151.1

```

#### Dzul (Sebagai Router)

```pascal
auto eth0
iface eth0 inet static
address 'IP_eth0_Dzul_tiap_kelompok'
netmask 255.255.255.252
gateway 'NID_tuntap_tiap_kelompok'

auto eth1
iface eth1 inet static
address 'IP_eth1_Dzul_tiap_kelompok'
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.0
```

#### Ardx (Sebagai DNS Server)

```pascal
auto eth0
iface eth0 inet static
address 'IP_Ardx_tiap_kelompok'
netmask 255.255.255.248
gateway 'IP_eth1_Dzul_tiap_kelompok'
```

#### Aldx (Sebagai Web Server)

```pascal
auto eth0
iface eth0 inet static
address 'IP_Aldx_tiap_kelompok'
netmask 255.255.255.248
gateway 'IP_eth1_Dzul_tiap_kelompok'
```

#### Walx (Sebagai  Klien)

```pascal
auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.0
gateway 192.168.0.1
```

#### Monx (Sebagai Klien)

```pascal
auto eth0
iface eth0 inet static
address 192.168.0.3
netmask 255.255.255.0
gateway 192.168.0.1
```

##### Keterangan :

- **IP_eth0_Dzul_tiap_kelompok** = NID_tuntap_tiap_kelompok + 2
- **IP_tuntap_tiap_kelompok** = NID_tuntap_tiap_kelompok + 1
- **IP_eth1_Dzul_tiap_kelompok** = NID_DMZ_tiap_kelompok + 1
- **IP_Ardx_tiap_kelompok** = NID_DMZ_tiap_kelompok + 2
- **IP_Aldx_tiap_kelompok** = NID_DMZ_tiap_kelompok + 3

#### Penjelasan Singkat :

- **IP Tuntap:** TUN yang merupakan kependekan dari Tunneling mensimulasikan layer 3, sedangkan TAP yang berarti Network Tap mensimulasikan layer 2. TUN berfungsi untuk routing, sedangkan TAP berfungsi sebagai network bridge.
- **Netmask:** Netmask adalah mask 32-bit yang digunakan untuk membagi alamat IP menjadi subnet dan menentukan host yang tersedia pada jaringan.
- **Gateway:** Jalur pada jaringan yang harus dilewati paket-paket data untuk dapat masuk ke jaringan yang lain.
- **DMZ:** DMZ adalah kependekan dari *Demilitarized Zone*, suatu area yang digunakan untuk berinteraksi dengan pihak luar. Di dalam jaringan komputer, DMZ merupakan suatu sub network yang terpisah dari sub network internal untuk keperluan keamanan.

  11. Restart network dengan mengetikkan `service networking restart` atau `/etc/init.d/networking restart` di setiap UM
  12. Coba cek IP pada setiap UML dengan mengetikkan `ifconfig`. Jika sudah mendapatkan IP seperti gambar di bawah, maka setting IP yang kalian lakukan sudah benar

      <img src="assets\BG10.png" alt="BG10" style="zoom:67%;" />
  13. Topologi yang dibuat sudah bisa berjalan secara lokal, tetapi kita belum bisa mengakses jaringan keluar. Ketikkan **`iptables-legacy –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16`** dan `iptables-legacy –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s NID_DMZ_tiap_kelompok/16` contoh `iptables-legacy –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.164.0/16` pada router Dzul.

      <img src="assets\BG11.png" alt="BG11" style="zoom:67%;" />

      ##### Keterangan :


      - jika belum menginstall iptables jalankan perintah `apt install iptables`
      - **Iptables:** iptables merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data. Dengan iptables inilah kita akan mengatur semua lalu lintas dalam komputer, baik yang masuk, keluar, maupun yang sekadar melewati komputer kita. Untuk penjelasan lebih lanjut nanti akan dibahas pada Modul 5.
      - **NAT (Network Address Translation):** Suatu metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan menggunakan satu alamat IP.
      - **Masquerade:** Digunakan untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.
      - **-s (Source Address):** Spesifikasi pada source. Address bisa berupa nama jaringan, nama host, atau alamat IP.
  14. Coba tes di semua UML dengan melakukan ping ke jaringan luar. Sebagai contoh coba `ping google.com` untuk mengecek apakah setting yang dilakukan sudah benar atau belum.

      <img src="assets\BG12.png" alt="BG12" style="zoom:67%;" />

      ##### **NOTE** : jika tidak bisa melakukan ping pada google.com, maka jalan perintah halt atau mengetikan bye.sh seperti pada cara 15, lalu jalankan ulang
  15. Terakhir, untuk mematikan UML **JANGAN langsung diclose**. Ketikkan `halt` pada setiap UML untuk mematikannya. Alternatif lain adalah dengan membuat script dengan ekstensi **.sh** supaya mempermudah serta mempercepat kalian untuk mematikannya. Sebagai contoh buat file bernama **bye.sh** dan tuliskan sintaks seperti di bawah ini:

      ```
      uml_mconsole Dzul halt
      uml_mconsole Walx halt
      uml_mconsole Monx halt
      uml_mconsole Ardx halt
      uml_mconsole Aldx halt
      ```

16. Simpan script yang sudah dibuat kemudian jalankan dengan mengetikkan `bash bye.sh`

    <img src="assets\BG13.png" alt="BG13" style="zoom: 80%;" />
