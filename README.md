# Jarkom-Shift-Modul-2-A01-2022

### Kelompok A01

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Fachrendy Zulfikar Abdillah  | 5025201018 | 
| 2 | Muhammad Dzikri Fakhrizal Syairozi | 5025201201 |
| 3 | Okyan Awang Ramadhana | 5025201252 |

https://docs.google.com/document/d/1bTtODp2nZwvdVZ-PV4ETgC8O93APC6nHpAGOaEQ-sWk/edit?usp=sharing

## Daftar isi

- [Anggota Kelompok](#anggota-kelompok)
- [Nomer 1](#nomer-1)
- [Nomer 2](#nomer-2)
- [Nomer 3](#nomer-3)
- [Nomer 4](#nomer-4)
- [Nomer 5](#nomer-5)
- [Nomer 6](#nomer-6)
- [Nomer 7](#nomer-7)
- [Nomer 8](#nomer-8)
- [Nomer 9](#nomer-9)
- [Nomer 10](#nomer-10)
- [Nomer 11](#nomer-11)
- [Nomer 12](#nomer-12)
- [Nomer 13](#nomer-13)
- [Nomer 14](#nomer-14)
- [Nomer 15](#nomer-15)
- [Nomer 16](#nomer-16)
- [Nomer 17](#nomer-17)
- [Kendala](#kendala)

## Nomer 1
- DNS master : WISE
- DNS Slave : Berlint
- Web server : Eden
- Client : SSS & Garden

Semua node terhubung pada router Ostania, sehingga dapat mengakses internet

> Konfigurasi Ostania
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.169.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.169.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.169.3.1
	netmask 255.255.255.0
```

> Konfigurasi WISE
```
auto eth0
iface eth0 inet static
	address 192.169.2.2
	netmask 255.255.255.0
	gateway 192.169.2.1
```

> Konfigurasi SSS
```
auto eth0
iface eth0 inet static
	address 192.169.1.2
	netmask 255.255.255.0
	gateway 192.169.1.1
```

> Konfigurasi Garden
```
auto eth0
iface eth0 inet static
	address 192.169.1.3
	netmask 255.255.255.0
	gateway 192.169.1.1
```

> Konfigurasi Berlint
```
auto eth0
iface eth0 inet static
	address 192.169.3.2
	netmask 255.255.255.0
	gateway 192.169.3.1
```

> Konfigurasi Eden
```
auto eth0
iface eth0 inet static
	address 192.169.3.3
	netmask 255.255.255.0
	gateway 192.169.3.1
```

> Dalam terminal Ostania
Masukkan command berikut. Prefix untuk kelompok kita adalah 192.169
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.169.0.0/16
apt-get update
```
command tersebut bisa dimasukkan ke dalam .bashrc

> Dalam terminal WISE, SSS, Garden, Berlint, dan Eden
Masukkan command berikut
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
```

Selanjutnya bisa dicoba `ping google.com` di setiap nodenya untuk mengetes apakah sudah terkoneksi ke internet
<img width="649" alt="1cpl" src="https://user-images.githubusercontent.com/32472207/198840325-ca152fd0-615e-4735-871c-00dacad1a3b9.png">


## Nomer 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise

> Dalam terminal WISE
Masukkan command berikut
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

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
echo 'nameserver 192.169.2.2' > /etc/resolv.conf
ping wise.a01.com -c 5
ping www.wise.a01.com -c 5
```
<img width="410" alt="2cpl" src="https://user-images.githubusercontent.com/32472207/198840336-94481ec3-fd1a-4256-bea6-47029e036a4b.png">


## Nomer 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

> Dalam terminal WISE
```
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

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
ping eden.wise.a01.com -c 5
ping www.eden.wise.a01.com -c 5
```
<img width="382" alt="3cpl" src="https://user-images.githubusercontent.com/32472207/198840341-e3d55b9f-b589-49a6-b9da-b8e9bd5bca90.png">


## Nomer 4
Buat juga reverse domain untuk domain utama

> Dalam terminal WISE
```
echo  'zone "2.169.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/2.169.192.in-addr.arpa";
};'  > /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/wise/2.169.192.in-addr.arpa
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
2.169.192.in-addr.arpa. IN      NS      wise.a01.com.
2       IN      PTR     wise.a01.com.
' > /etc/bind/wise/2.169.192.in-addr.arpa
service bind9 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
echo  '
nameserver 192.168.122.1
'  > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
echo  '
nameserver 192.169.2.2
'  > /etc/resolv.conf
host -t PTR 192.169.2.2
```
<img width="555" alt="4cpl" src="https://user-images.githubusercontent.com/32472207/198840353-eafc558d-0cd8-4f96-97f3-c4d7cee758c7.png">


## Nomer 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

> Dalam terminal WISE
```
echo  'zone "wise.a01.com" {
        type master;
	   notify yes;
	   also-notify { 192.169.3.2; };
	   allow-transfer { 192.169.3.2; };
        file "/etc/bind/wise/wise.a01.com";
};' > /etc/bind/named.conf.local
service bind9 restart
```

> Dalam terminal Berlint
```
apt-get install bind9 -y
echo  'zone "wise.a01.com" {
        type slave;
	   masters { 192.169.2.2; };
   file "/var/lib/bind/wise.a01.com";
};' > /etc/bind/named.conf.local
service bind9 restart
```

> Dalam terminal WISE
```
service bind9 stop
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
echo  '
nameserver 192.169.3.2
nameserver 192.169.2.2
'  > /etc/resolv.conf
ping wise.a01.com -c 5
```
<img width="398" alt="5cpl" src="https://user-images.githubusercontent.com/32472207/198840359-ad55c9cc-83a5-4ad4-8f60-d78b1888d945.png">


## Nomer 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation

> Dalam terminal WISE
```
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
ns2                     IN      A       192.169.3.2
operation               IN      A       ns2
www.operation           IN      CNAME   operation
' > /etc/bind/wise/wise.a01.com
service bind9 restart

echo  '
options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
```

> Dalam terminal Berlint
```
echo '
options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

echo  'zone "operation.wise.a01.com" {
        type master;
        file "/etc/bind/operation/operation.wise.a01.com";
};
zone "wise.a01.com" {
        type slave;
	   masters { 192.169.2.2; };
   file "/var/lib/bind/wise.a01.com";
};
' > /etc/bind/named.conf.local
mkdir /etc/bind/operation
cp /etc/bind/db.local /etc/bind/operation/operation.wise.a01.com
echo  '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.a01.com. root.operation.wise.a01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@                       IN      NS      operation.wise.a01.com.
@                       IN      A       192.169.3.3
www                     IN      CNAME   operation.wise.a01.com.

' > /etc/bind/operation/operation.wise.a01.com
service bind9 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
    ping operation.wise.a01.com -c 5
    ping www.operation.wise.a01.com -c 5

```
<img width="455" alt="6cpl" src="https://user-images.githubusercontent.com/32472207/198840381-7f65ec84-fae8-444a-9f84-a450c6773a44.png">


## Nomer 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden 

> Dalam terminal Berlint
```
echo  '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.a01.com. root.operation.wise.a01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@                       IN      NS      operation.wise.a01.com.
@                       IN      A       192.169.3.3
www                     IN      CNAME   operation.wise.a01.com.
strix                   IN      A       192.169.3.3
www.strix               IN      CNAME   strix
' > /etc/bind/operation/operation.wise.a01.com
service bind9 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
ping strix.operation.wise.a01.com -c 5
ping www.strix.operation.wise.a01.com -c 5
```
<img width="447" alt="7cpl" src="https://user-images.githubusercontent.com/32472207/198840396-b5a2ff42-b435-4b5b-ab28-bdf1beb996b1.png">


## Nomer 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com

> Dalam terminal Wise
```
cp /etc/bind/wise/2.169.192.in-addr.arpa /etc/bind/wise/3.169.192.in-addr.arpa
echo '
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
3.169.192.in-addr.arpa. IN      NS      wise.a01.com.
2       IN      PTR     wise.a01.com.
' > /etc/bind/wise/3.169.192.in-addr.arpa

echo '
zone "wise.a01.com" {
        type master;
        file "/etc/bind/wise/wise.a01.com";
	   allow-transfer { 192.169.3.2; };
};

zone "3.169.192.in-addr.arpa" {
        type master;
        file "/etc/bind/wise/3.169.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local

echo '
$TTL    604800
@       IN      SOA     wise.a01.com. root.wise.a01.com.(
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      wise.a01.com.
@       IN      A       192.169.3.3
www     IN      CNAME   wise.a01.com.
eden    IN      A       192.169.3.3
www.eden IN CNAME eden.wise.a01.com.
ns1     IN      A       192.169.3.3
operation  IN  NS  ns1
' > /etc/bind/wise/wise.a01.com

service bind9 restart
```

> Dalam terminal Eden
```
apt-get update
apt-get install apache2 -y
service apache2 start
apt install php -y
apt install libapache2-mod-php7.0 -y
apt install wget -y
apt install unzip -y

cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wise.a01.com.conf 

echo '
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
   DocumentRoot /var/www/wise.a01.com
   ServerName wise.a01.com
        ServerAlias www.wise.a01.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
' > /etc/apache2/sites-available/wise.a01.com.conf
mkdir /var/www/wise.a01.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1S0XhL 9ViYN7TyCj2W66BNEXQD2AAAw2e' -O /var/www/wise.zip
unzip /var/www/wise.zip -d /var/www
cp /var/www/wise/* /var/www/wise.a01.com
a2ensite wise.a01.com
service apache2 restart
service apache2 reload
```


Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
apt-get install lynx
echo “nameserver 192.169.2.2” > /etc/resolv.conf
lynx www.wise.a01.com
```
<img width="559" alt="8cpl" src="https://user-images.githubusercontent.com/32472207/198840418-a782ba87-6d81-4050-b363-904cd52d37af.png">


## Nomer 9
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home

> Dalam terminal Eden
```
a2enmod rewrite
service apache2 restart 
echo '
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule (.*) /index.php/\$1 [L]
' > /var/www/wise.a01.com/.htaccess 
echo '
<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/wise.a01.com
ServerName wise.a01.com
ServerAlias www.wise.a01.com 
ErrorLog \${APACHE_LOG_DIR}/error.log
CustomLog \${APACHE_LOG_DIR}/access.log combined
<Directory /var/www/wise.a01.com>
Options +FollowSymLinks -Multiviews
AllowOverride All
</Directory>
</VirtualHost>

' > /etc/apache2/sites-available/wise.a01.com.conf service apache2 restart 
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx www.wise.a01.com/home
```
<img width="559" alt="8cpl" src="https://user-images.githubusercontent.com/32472207/198840454-db0d41e9-25d1-47cc-ab97-2c29efbe2ca9.png">


## Nomer 10
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com

> Dalam terminal Eden
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/eden.wise.a01.com.conf

echo '
<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/eden.wise.a01.com
ServerName eden.wise.a01.com
ServerAlias www.eden.wise.a01.com
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
<VirtualHost>
' > /etc/apache2/sitesavailable/eden.wise.a01.com.conf
mkdir /var/www/eden.wise.a01.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1q9g6n M85bW5T9f5yoyXtDqonUKKCHOTV' -O /var/www/eden.wise.zip unzip /var/www/eden.wise.zip -d /var/www
cp /var/www/eden.wise/* /var/www/eden.wise.a01.com
a2ensite eden.wise.a01.com
service apache2 restart
service apache2 reload
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx www.eden.wise.a01.com
```
<img width="551" alt="10" src="https://user-images.githubusercontent.com/32472207/198840461-7a873fde-d744-4528-99a4-b2879ed78e74.png">


## Nomer 11
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja

> Dalam terminal Eden
```
echo "
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.a01.com
        ServerName eden.wise.a01.com
        ServerAlias www.eden.wise.a01.com

        <Directory /var/www/eden.wise.a01.com/public>
                Options +Indexes
        </Directory>

          <Directory /var/www/eden.wise.a01.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
         </Directory>

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log
combined
</VirtualHost>
" > /etc/apache2/sites-available/eden.wise.a01.com.conf

service apache2 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx eden.wise.a01.com/public
```
<img width="556" alt="11" src="https://user-images.githubusercontent.com/32472207/198840465-0d5f1551-4722-479f-9b5b-357f6447fb85.png">


## Nomer 12
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache

> Dalam terminal Eden
```
echo "
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.a01.com
        ServerName eden.wise.a01.com
        ServerAlias www.eden.wise.a01.com

        ErrorDocument 404 /error/404.html
        ErrorDocument 500 /error/404.html
        ErrorDocument 502 /error/404.html
        ErrorDocument 503 /error/404.html
        ErrorDocument 504 /error/404.html

        <Directory /var/www/eden.wise.a01.com/public>                                   
 Options +Indexes
        </Directory>

         <Directory /var/www/eden.wise.a01.com>
                Options +FollowSymLinks -Multiviews             
 AllowOverride All
         </Directory>

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log
combined 
</VirtualHost>
" > /etc/apache2/sitesavailable/eden.wise.a01.com.conf

service apache2 restart

service apache2 reload
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx eden.wise.a01.com/anythingexceptjarkom
```
<img width="557" alt="12" src="https://user-images.githubusercontent.com/32472207/198840473-d08b4f26-bcf6-43fe-a9be-1402fa978f5e.png">


## Nomer 13
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js 

> Dalam terminal Eden
```
echo '
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.a01.com
        ServerName eden.wise.a01.com
        ServerAlias www.eden.wise.a01.com

        <Directory /var/www/eden.wise.a01.com/public>                Options +Indexes
        </Directory>

        Alias "/js"
"/var/www/eden.wise.a01.com/public/js"

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log
combined

        ErrorDocument 404 /error/404.html
        ErrorDocument 500 /error/404.html
        ErrorDocument 502 /error/404.html
        ErrorDocument 503 /error/404.html
        ErrorDocument 504 /error/404.html

        <Directory /var/www/eden.wise.a01.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
</VirtualHost>
'  > /etc/apache2/sites-available/eden.wise.a01.com.conf

service apache2 restart

service apache2 reload

```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx www.eden.wise.a01.com/js
```
<img width="552" alt="13" src="https://user-images.githubusercontent.com/32472207/198840481-4336f41c-664c-4a51-84e7-33046c5bc497.png">


## Nomer 14
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500

> Dalam terminal Eden
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/eden.wise.a01.com.conf
echo '
<VirtualHost *:15000 *:15500>
     ServerAdmin webmaster@localhost
DocumentRoot /var/www/strix.operation.wise.a01.com
ServerName strix.operation.wise.a01.com
ServerAlias www.strix.operation.wise.a01.com
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog        ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' > /etc/apache2/sites-
available/strix.operation.wise.a01.com.conf
mkdir /var/www/strix.operation.wise.a01.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT’ -O /var/www/strix.operation.wise.zip
unzip /var/www/strix.operation.wise.zip -d /var/www cp -r /var/www/strix.operation.wise/* /var/www/strix.operation.wise.a01.com
rm -r /var/www/strix.operation.wise
a2ensite strix.operation.wise.a01.com

echo '
Listen 80
Listen 15000
Listen 15500

<IfModule ssl_module>
Listen 443
</IfModule>

<IfModule gnutls.c>
Listen 443
</IfModule>
' > /etc/apache2/ports.conf
service apache2 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
echo ‘nameserver 192.169.2.2 #IP WISE nameserver 192.169.3.2 # IP Berlint’ > /etc/resolv.conf
lynx strix.operation.wise.a01.com:15000
lynx strix.operation.wise.a01.com:15500
```
<img width="560" alt="14a" src="https://user-images.githubusercontent.com/32472207/198840495-e228f1c8-aaee-49d8-9833-e7947664137d.png">

## Nomer 15
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy

> Dalam terminal Eden
```
apt-get install apache2-utils
htpasswd -c /etc/apache2/.htpasswd Twilight opStrix 
echo '
<VirtualHost *:15000 *:15500>
ServerAdmin webmaster@localhost
DocumentRoot
/var/www/strix.operation.wise.a01.com
ServerName strix.operation.wise.a01.com
ServerAlias www.strix.operation.wise.a01.com

<Directory
"/var/www/strix.operation.wise.a01.com"> AuthType Basic
AuthName "Restricted Content"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
</Directory>

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog        ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' > /etc/apache2/sites-available/strix.operation.wise.a01.com.conf
service apache2 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
echo ‘nameserver 192.169.2.2 #IP WISE nameserver
192.175.3.2 # IP Berlint’ > /etc/resolv.conf
lynx strix.operation.wise.a01.com:15000 atau :15500

```
<img width="560" alt="14a" src="https://user-images.githubusercontent.com/32472207/198840528-5e59fcac-1821-43dd-8d3e-48afe440d767.png">


## Nomer 16
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com

> Dalam terminal Eden
```
echo '
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        RewriteEngine On
        RewriteCond %{HTTP_HOST} !^wise.a01.com$
        RewriteRule /.* http://wise.a01.com/ [R]

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog 
\${APACHE_LOG_DIR}/access.log 
combined

</VirtualHost>
' > /etc/apache2/sites-available/000-default.conf
service apache2 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
lynx 192.169.3.3
```
<img width="556" alt="16a" src="https://user-images.githubusercontent.com/32472207/198840539-68a92b9e-f064-4f0d-9bab-9b8337c76031.png">
<img width="554" alt="16b" src="https://user-images.githubusercontent.com/32472207/198840542-986e56f6-0a25-409f-9d8b-64dc59cc43f2.png">


## Nomer 17
arena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian! 

> Dalam terminal Eden
```
a2enmod rewrite
echo '
RewriteEngine On
RewriteRule (.*)eden(.*)(\.jpg|\.png|\.gif)$
http://eden.wise.a01.com/public/images/eden.png
[L,R=301]
' > /var/www/eden.wise.a01.com/.htaccess

echo '
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.a01.com
        ServerName eden.wise.a01.com
        ServerAlias www.eden.wise.a01.com

        ErrorDocument 404 /error/404.html
        ErrorDocument 500 /error/404.html
        ErrorDocument 502 /error/404.html
        ErrorDocument 503 /error/404.html
        ErrorDocument 504 /error/404.html

        <Directory /var/www/eden.wise.a01.com/public>
                Options +Indexes
        </Directory>

        Alias 
"/js" 
"/var/www/eden.wise.a01.com/public/js"

        <Directory /var/www/eden.wise.a01.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog 
\${APACHE_LOG_DIR}/access.log 
combined

</VirtualHost>
' > /etc/apache2/sites-available/eden.wise.a01.com.conf
service apache2 restart
```

Untuk mengetest hasilnya
> Dalam terminal SSS & Garden
```
#testing
lynx eden.wise.a01.com/public/images/not-eden.png 
```
<img width="568" alt="17cpl" src="https://user-images.githubusercontent.com/32472207/198840574-416da99d-c32d-4d02-8bcf-3f38ea048ddb.png">

