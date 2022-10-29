# Jarkom-Shift-Modul-2-A01-2022

### Kelompok A01

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Fachrendy Zulfikar Abdillah  | 5025201018 | 
| 2 | Muhammad Dzikri Fakhrizal Syairozi | 5025201201 |
| 3 | Okyan Awang Ramadhana | 5025201252 |

https://docs.google.com/document/d/1bTtODp2nZwvdVZ-PV4ETgC8O93APC6nHpAGOaEQ-sWk/edit?usp=sharing

<h2>Daftar Isi</h2>

- [Soal 1](#soal-1) <br>
- [Soal 2](#soal-2) <br>
- [Soal 3](#soal-3) <br>
- [Soal 4](#soal-4) <br>
- [Soal 5](#soal-5) <br>
- [Soal 6](#soal-6) <br>
- [Soal 7](#soal-7) <br>
- [Soal 8](#soal-8) <br>
- [Soal 9](#soal-9) <br>
- [Soal 10](#soal-10) <br>
- [Soal 11](#soal-11) <br>
- [Soal 12](#soal-12) <br>
- [Soal 13](#soal-13) <br>
- [Soal 14](#soal-14) <br>
- [Soal 15](#soal-15) <br>
- [Soal 16](#soal-16) <br>
- [Soal 17](#soal-17) <br>

<h3>Soal 1</h3>
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet (1).
<ul>
  <li>[On Wise]</li>
  ```
  
  ```
  <li>[On SSS & Garden]</li>
  ```
  
  ```
</ul>

<h3>Soal 2</h3>
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise 
<b>Langkah pengerjaan : </b>
<ul>
  <li>[On Wise]</li>
  ```
  apt-get install bind9 -y
echo  'zone "wise.a01.com" {
        type master;
        file "/etc/bind/wise/wise.a01.com";
};'  > /etc/bind/named.conf.local
mkdir /etc/bind/wise
cp /etc/bind/db.local /etc/bind/wise/wise.a01.com
echo  '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.a01.com. root.wise.a01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@                       IN      NS      wise.a01.com.
@                       IN      A       192.169.2.2
www                     IN      CNAME   wise.a01.com.
eden                    IN      A       192.169.3.3
www.eden                IN      CNAME   eden
' > /etc/bind/wise/wise.a01.com
service bind9 restart
  ```
  <li>[On SSS & Garden]</li>
  ```
  
  ```
</ul>

<h3>Soal 3</h3>
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>
<img src="assets/3.png" alt="3">

<h3>Soal 4</h3>
Buat juga reverse domain untuk domain utama
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 5</h3>
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 6</h3>
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 7</h3>
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden
<b>Langkah pengerjaan : </b>
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 8</h3>
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 9</h3>
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 10</h3>
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 11</h3>
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 12</h3>
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 13</h3>
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 14</h3>
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 15</h3>
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 16</h3>
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>

<h3>Soal 17</h3>
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian!
<ul>
  <li>[On Wise]</li>
  <img src="assets/1.png" alt="1">
  <li>[On SSS & Garden]</li>
  <img src="assets/1.png" alt="1">
</ul>
