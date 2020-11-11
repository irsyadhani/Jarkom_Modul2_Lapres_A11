# Jarkom_Modul2_Lapres_A11
Kelompok A11:
* _Irsyadhani Dwi Shubhi (0511184000022)_
* _Anggun Wahyuni (05111840000154)_

----------------------------------------------------------------
## Soal
* [DNS](#dns)
  * [Soal 1](#soal-1)
  * [Soal 2](#soal-2)
  * [Soal 3](#soal-3)
  * [Soal 4](#soal-4)
  * [Soal 5](#soal-5)
  * [Soal 6](#soal-6)
  * [Soal 7](#soal-7)
* [Web Sever](#web-server)
  * [Soal 8](#soal-8)
  * [Soal 9](#soal-9)
  * [Soal 10](#soal-10)
  * [Soal 11](#soal-11)
  * [Soal 12](#soal-12)
  * [Soal 13](#soal-13)
  * [Soal 14](#soal-14)
  * [Soal 15](#soal-15)
  * [Soal 16](#soal-16)
  * [Soal 17](#soal-17)
----------------------------------------------------------------
# DNS
Kalian diminta untuk membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw yang
memiliki (2) alias http://www.semeruyyy.pw, dan (3) subdomain
http://www.penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP
Server PROBOLINGGO serta dibuatkan (4) reverse domain untuk domain utama. Untuk mengantisipasi
server dicuri/rusak, Bibah minta dibuatkan (5) DNS Server Slave pada MOJOKERTO agar Bibah tidak
terganggu menikmati keindahan Semeru pada Website. Selain website utama Bibah juga meminta
dibuatkan (6) subdomain dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server
MOJOKERTO dan mengarah ke IP Server PROBOLINGGO. Bibah juga ingin memberi petunjuk
mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan (7) subdomain
dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

#### Soal 1:
_**Penyelesaian:**_
● Lakukan perintah pada MALANG
```
nano /etc/bind/named.conf.local
```
● Isikan configurasi domain semerua11.pw sesuai dengan syntax berikut:
```
zone "semerua11.pw" {
	type master;
	file "/etc/bind/jarkom/semerua11.pw";
};
```
● Buat folder jarkom di /etc/bind
● Copykan file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi semerua11.pw
```
cp /etc/bind/db.local /etc/bind/jarkom/semerua11.pw
```
● Kemudian buka file semerua11.pw dan edit seperti gambar berikut dengan IP PROBOLINGGO
```
nano /etc/bind/jarkom/semerua11.pw
```
![alt text](/img/1.PNG)

● Restart bind9 dengan perintah
```
service bind9 restart
```
● Pada client GRESIK dan SIDOARJO arahkan nameserver menuju IP PROBOLINGGO dengan mengedit file resolv.conf dengan mengetikkan perintah
 ```
 nano /etc/resolv.conf
 ```
 ![alt text](/img/sidoarjo tes.png)
● Untuk mencoba koneksi DNS, lakukan ping domain semerua11.pw dengan melakukan perintah berikut pada client GRESIK dan SIDOARJO
```
ping semerua11.pw
```
 ![alt text](/img/1 result.png)
#
#### Soal 2:
_**Penyelesaian:**_
● Buka file semerua11.pw pada server MALANG dan tambahkan konfigurasi seperti pada gambar berikut:
![alt text](/img/2.png)

● Kemudian restart bind9 dengan perintah
```
service bind9 restart
```
● Lalu cek dengan melakukan ping www.semerua11.pw
![alt text](/gambar/soal2c.png)
#
#### Soal 3:
_**Penyelesaian:**_
● Edit file /etc/bind/jarkom/semerua11.pw lalu tambahkan subdomain untuk semerua11.pw yang mengarah ke IP PROBOLINGGO
```
nano /etc/bind/jarkom/semerua11.pw
```
● Tambahkan konfigurasi seperti pada gambar ke dalam file semerua11.pw.
![alt text](/img/3 cropped.png)
● Restart service bind
```
service bind9 restart
```
● Ping ke subdomain dengan perintah berikut dari client GRESIK
```
ping penanjakan.semerua11.pw
```
![alt text](/gambar/3 result.png)
#
#### Soal 4:
_**Penyelesaian:**_
● Edit file /etc/bind/named.conf.local pada MALANG
```
nano /etc/bind/named.conf.local
```
● Lalu tambahkan konfigurasi berikut ke dalam file named.conf.local
```
zone "73.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/73.151.10.in-addr.arpa";
};
```
● Copykan file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi 73.151.10.in-addr.arpa
```
cp /etc/bind/db.local /etc/bind/jarkom/73.151.10.in-addr.arpa
```
● Edit file 71.151.10.in-addr.arpa menjadi seperti gambar di bawah ini
![alt text](/gambar/4.png)
● Kemudian restart bind9 dengan perintah
```
service bind9 restart
```
#
#### Soal 5:
_**Penyelesaian:**_
● Edit file /etc/bind/named.conf.local dan sesuaikan dengan syntax berikut (SERVER MALANG)
```
zone "semerua11.pw" {
    type master;
    notify yes;
    also-notify { 10.151.73.99; }; // IP MOJOKERTO
    allow-transfer { 10.151.73.99; }; 
    file "/etc/bind/jarkom/jarkom2020.com";
};
```
![alt text](/img/5.png)
● Restart service bind
```
service bind9 restart
```
● Kemudian buka file /etc/bind/named.conf.local pada MOJOKERTO dan tambahkan syntax berikut:
```
zone "semerua11.pw" {
    type slave;
    masters { 10.151.73.98; }; // IP MALANG
    file "/var/lib/bind/jarkom2020.com";
};
```
![alt text](/gambar/5b.png)
● Restart service bind
```
service bind9 restart
```
● Pada server MALANG matikan service bind9 (untuk testing)
```
service bind9 stop
```
● ping semerua11.pw
![alt text](/gambar/5 result.png)
#
#### Soal 6 dan 7:
_**Penyelesaian:**_
● Pada MALANG, edit file /etc/bind/jarkom/semerua11.pw
```
nano /etc/bind/jarkom/semerua11.pw
```
![alt text](/img/6.png)
● Kemudian edit file /etc/bind/named.conf.options pada MALANG.
```
nano /etc/bind/named.conf.options
```
Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
```
allow-query{any;};
```
![alt text](/img/6b.png)
● Kemudian edit file /etc/bind/named.conf.local
![alt text](/img/6cc.png)
● Setelah itu restart bind9
● Pada MOJOKERTO edit file /etc/bind/named.conf.options
● Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
```
allow-query{any;};
```
![alt text](/img/6d.png)
● Lalu edit file /etc/bind/named.conf.local menjadi seperti gambar di bawah:
![alt text](/img/6e.png)
● Kemudian buat direktori dengan nama delegasi
● Copy db.local ke direktori delegasi dan edit namanya menjadi gunung.semerua11.pw
● Kemudian edit file gunung.semerua11.pw menjadi seperti dibawah ini
![alt text](/img/6f.png)
● Restart bind9
● Testing : Lakukan ping ke domain gunung.semerua11.pw dan naik.gunung.semerua11.pw dari client GRESIK
![alt text](/img/7.png)

# Web Server
#### Soal 8:
_**Penyelesaian:**_
● Pada PROBOLINGGO, Pindah ke directory /etc/apache2/sites-available
Copy file default menjadi file semerua11.pw.
● Buka file semerua11.pw
● Tambahkan
```
ServerName semerua11.pw
ServerAlias semerua11.pw
```
● Ubah DocumentRoot menjadi /var/www/semerua11.pw
![alt text](/img/8.png)
● Gunakan perintah a2ensite semerua11.pw
● Gunakan perintah service apache2 restart
● Pindah ke directory /var/www 
● Gunakan perintah wget 10.151.36.202/semeru.pw.zip
● Unzip file yang telah di download dan ubah namanya menjadi semerua11.pw
● Buka browser dan akses http://semerua11.pw
![alt text](/img/8 result.png)
#
#### Soal 9:
_**Penyelesaian:**_
● Jalankan perintah a2enmod rewrite
● Restart apache dengan perintah service apache2 restart
● Pindah ke directory /var/www/semerua11.pw dan buat file .htaccess dengan isi file
![alt text](/img/9 ht.png)
● Pindah ke directory /etc/apache2/sites-available kemudian buka file semerua11.pw dan tambahkan
```
 <Directory /var/www/semerua11.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
 </Directory>
 ```
 ![alt text](/img/9.png)
 ● Restart apache dengan perintah service apache2 restart
 ● Buka browser dan akses http://semerua11.pw/home
 ![alt text](/img/9 before.png)
 ![alt text](/img/9 after.png)
#
#### Soal 10:
_**Penyelesaian:**_
● Pada PROBOLINGGO, Pindah ke directory /etc/apache2/sites-available
Copy file default menjadi file penanjakan.semerua11.pw.
● Buka file penanjakan.semerua11.pw
● Tambahkan
```
ServerName penanjakan.semerua11.pw
ServerAlias penanjakan.semerua11.pw
```
● Ubah DocumentRoot menjadi /var/www/penanjakan.semerua11.pw
![alt text](/img/10.png)
● Gunakan perintah a2ensite penanjakan.semerua11.pw
● Gunakan perintah service apache2 restart
● Pindah ke directory /var/www 
● Gunakan perintah wget 10.151.36.202/penanjakan.semeru.pw.zip
● Unzip file yang telah di download dan ubah namanya menjadi penanjakan.semerua11.pw
#
#### Soal 11:
_**Penyelesaian:**_
● Pindah ke directory /etc/apache2/sites-available kemudian buka file penanjakan.semerua11.pw dan tambahkan
```
 <Directory /var/www/penanjakan.semerua11.pw/public>
     Options +Indexes
 </Directory>
 <Directory /var/www/penanjakan.semerua11.pw/public/javascripts>
     Options -Indexes
 </Directory>
 <Directory /var/www/penanjakan.semerua11.pw/public/css>
     Options -Indexes
 </Directory>
 <Directory /var/www/penanjakan.semerua11.pw/public/images>
     Options -Indexes
 </Directory>
```
![alt text](/img/11.png)
● Restart apache dengan perintah service apache2 restart
● Hasil :
![alt text](/img/11 public.png)
![alt text](/img/11 css.png)
![alt text](/img/11 js.png)
![alt text](/img/11 images.png)
#
#### Soal 12:
_**Penyelesaian:**_
● Pindah ke directory /etc/apache2/sites-available kemudian buka file penanjakan.semerua11.pw dan tambahkan
```
ErrorDocument 404 /errors/404.html
```
![alt text](/img/12.png)
● Hasilnya
![alt text](/img/12 result.png)
#
#### Soal 13:
_**Penyelesaian:**_
● Pindah ke directory /etc/apache2/sites-available kemudian buka file penanjakan.semerua11.pw dan tambahkan
```
Alias /js /var/www/penanjakan.semerua11.pw/public/javascripts
```
![alt text](/img/13.png)
● Hasilnya
![alt text](/img/13 result.png)
#
#### Soal 14:
_**Penyelesaian:**_
● Pindah ke directory /etc/apache2/sites-available
Copy file default menjadi file naik.gunung.semerua11.pw.
● Edit file naik.gunungsemerua11.pw
![alt text](/img/14.png)
● Tambahkan directory listing agar website dapat dilihat
![alt text](/img/14c.png)
● Tambahkan port 8080 pada file ports.conf
![alt text](/img/14b.png)
● Gunakan perintah a2ensite naik.gunung.semerua11.pw
● Gunakan perintah service apache2 restart
● Pindah ke directory /var/www 
● Gunakan perintah wget 10.151.36.202/naik.gunung.semeru.pw.zip
● Unzip file yang telah di download di folder naik.gunung.semeru.pw
● Hasilnya
![alt text](/img/14 result.png)
#
#### Soal 15:
_**Penyelesaian:**_
● install apache utilities package
```
apt-get update
apt-get install apache2 apache2-utils
```
● Buat file password, masukkan username dan password
```
htpasswd -c /etc/apache2/.htpasswd semeru
```
![alt text](/img/15b.png)
● Edit file /etc/apache2/sites-enabled/naik.gunung.semerua11.pw seperti gambar
![alt text](/img/15c.png)
● restart apache
● hasilnya
![alt text](/img/15 result.png)
#
#### Soal 16:
_**Penyelesaian:**_
● Pindah ke directory /var/www/ dan buat file .htaccess dengan isi file seperti gambar
![alt text](/img/16.png)
● restart apache
● Buka browser dan akses ke 10.151.73.100 akan auto redirect ke semerua11.pw
![alt text](/img/16 before.png)
![alt text](/img/16 after.png)
#
#### Soal 17:
_**Penyelesaian:**_
● Pindah ke directory /var/www/penanjakan.semerua11.pw dan buat file .htaccess dengan isi file seperti gambar
![alt text](/img/17.png)
● restart apache
● Buka browser dan akses ke penanjakan.semerua11.pw/public/images/tessemeru.jpg akan auto redirect ke  penanjakan.semerua11.pw/public/images/semeru.jpg
![alt text](/img/17 before.png)
![alt text](/img/17 after.png)
#
