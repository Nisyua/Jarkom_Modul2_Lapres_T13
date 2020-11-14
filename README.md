# Jarkom_Modul2_Lapres_T13
---

* Anis Saidatur Rochma 05311840000002
* Desya Ananda Puspita Dewi 05311840000046

### Requirements :

**Install [Putty](https://www.putty.org/),[OpenVPN](https://openvpn.net/), dan [Xming](https://sourceforge.net/projects/xming/)**


*__SOAL No. 1__*
---
### Membuat sebuah website utama dengan alamat http://semerut13.pw yang memiliki 

  * Buka MALANG dan update package lists dengan menjalankan command: `apt-get update`
  * Lakukan installasi aplikasi bind9 pada Malang `apt-get install bind9 -y`
  * Lakukan perintah pada MALANG ` nano /etc/bind/named.conf.local`
  * Isikan configurasi domain semerut13.pw sesuai dengan syntax berikut:
  ```
zone "jarkom2020.com" {
	type master;
	file "/etc/bind/jarkom/semerut13.pw";
};
  ```
  
  ![](/images/1-1.png)
  
  * Buat folder  di dalam `/etc/bind/jarkom`
  * Copykan file `db.local` pada path `/etc/bind` ke dalam folder jarkom yang baru saja dibuat :
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/semerut13.pw
  ```
  * Buka dan edit file semerut13.pw dengan perintah : 
  ```
  nano /etc/bind/jarkom/semerut13.pw
  ```
  
   ![](/images/1-2.png)
   
  * Lalu restart bind9 dengan :
  ```
  service bind9 restart
  ```
  * Pada client GRESIK dan SIDOARJO arahkan nameserver menuju IP MALANG dengan mengedit file resolve.conf dengan perintah
  ```
  nano /etc/resolv.conf
  ```
  * Untuk mencoba koneksi DNS, lakukan ping domain jarkom2020.com dengan melakukan perintah berikut pada client **GRESIK** dan **SIDOARJO** 
  ```
  ping semerut13.pw
  ```
   ![](/images/1-3.png)
  
*__SOAL No. 2__*
---
### alias http://www.semerut13.pw

  * menambahkan `cname` dengan menuliskan `www`
  ```
  www   IN   CNAME   semerut13.pw
  ```
   ![](/images/2-1.png)
   
  * Kemudian restart bind9 dengan perintah

```
service bind9 restart
```
* Lalu cek di client **GRESIK** dengan perintah `ping www.semerut13.pw`
 
 ![](/images/2-2.png)

*__SOAL No. 3__*
---
### subdomain http://penanjakan.semerut13.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO serta dibuatkan

* Edit file pada Malang `/etc/bind/jarkom/semerut13.pw` lalu tambahkan seperti dibawah :
```
@	          IN	  A	      10.151.77.156	; IP PROBOLINGGO
www	          IN	  CNAME       semerut13.pw.
penanajakan	  IN	  A	      10.151.77.156	; IP PROBOLINGGO
```

 ![](/images/3-1.png)
 
* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Pergi ke **GRESIK** dan lakukan testing dengan perintah `ping penanjakan.semerut13.pw`

 ![](/images/3-2.png)

*__SOAL No. 4__*
---
### reverse domain untuk domain utama. Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan

  * Edit file `/etc/bind/named.conf.local` pada ***MALANG***
  ```
  nano /etc/bind/named.conf.local
  ```
  * Lalu tambahkan konfigurasi berikut ke dalam file `named.conf.local`
  ```
  zone "77.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/77.151.10.in-addr.arpa";
};
  ```
  
   ![](/images/4-1.png)
   
  * Copykan file db.local pada path /etc/bind ke dalam folder jarkom dengan perintah
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/77.151.10.in-addr.arpa
  ```
  * Kemudian edit file dengan `nano /etc/bind/jarkom/77.151.10.in-addr.arpa`
  
   ![](/images/4-2.png)
   
  * Restart bind9 dengan `service bind9 restart`
  * Pergi ke **GRESIK** dan lakukan testing dengan perintah `host -t PTR [IP PROBOLINGGO]`
  
  ![](/images/4-3.png)
  
*__SOAL No. 5__*
---

### DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website. Selain website utama Bibah juga meminta dibuatkan

  * Edit file `/etc/bind/named.conf.local` pada ***MALANG***
  ```
  nano /etc/bind/named.conf.local
  ```
  * Lalu tambahkan konfigurasi berikut ke dalam file `named.conf.local`
  ```
  zone "semerut13.pw" {
    type master;
    notify yes;
    also-notify { 10.151.77.155; }; // Masukan IP MOJOKERTO tanpa tanda petik
    allow-transfer { 10.151.77.155; }; // Masukan IP MOJOKERTO tanpa tanda petik
    file "/etc/bind/jarkom/semerut13.pw";
    };
  ```
  
  ![](/images/5-1.png)
  
  * Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
  * Pergi ke ***MOJOKERTO*** dan jalankan perintah `apt-get update` kemudian `apt-get install bind9 -y
  * Edit file `/etc/bind/named.conf.local` pada ***MOJOKERTO***
  ```
  nano /etc/bind/named.conf.local
  ```
  * Lalu tambahkan konfigurasi berikut ke dalam file `named.conf.local`
  ```
  zone "semerut13.pw" {
    type slave;
    masters { 10.151.77.154; } // Masukan IP MALANG tanpa tanda petik
    file "/var/lib/bind/semerut13.pw";
  };
  ```
    ![](/images/5-2.png)
    
  * Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
  * Pada server ***MALANG*** matikan service bind dengan perintah `service bind9 stop`
  * Pastikan nameserver pada **GRESIK** mengarah ke *IP MOJOKERTO* dan *IP MALANG*
  * Lalu cek di client **GRESIK** dengan perintah `ping semerut13.pw`
    
    ![](/images/5-3.png)

*__SOAL No. 6__*
---

### Subdomain dengan alamat http://gunung.semerut13.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO. Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan

* Edit file pada Malang `/etc/bind/jarkom/semerut13.pw` lalu tambahkan seperti dibawah :
  ```
  @	          IN	  A	      10.151.77.156	; IP PROBOLINGGO
  www	          IN	  CNAME       semerut13.pw.
  penanajakan	  IN	  A	      10.151.77.156	; IP PROBOLINGGO
  ns1		  IN	  A	      10.151.77.155	; IP MOJOKERTO
  gunung		  IN	  NS	      ns1
  ```
  ![](/images/6-1.png)
 
* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Edit file pada Malang `/etc/bind/named.conf.options` dengan melakukan comment **dnssec-validation auto** dan tambahkan 
  ```
  allow-query{any;};
  ```
* Edit file `/etc/bind/named.conf.local` pada ***MALANG***
  ```
  zone "semerut13.pw" {
    type master;
    file "/etc/bind/jarkom/semerut13.pw";
    allow-transfer { 10.151.77.155; }; // Masukan IP MOJOKERTO tanpa tanda petik
  };
  ```
  ![](/images/6-2.png)
  
* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Pergi ke ***MOJOKERTO*** dan edit file `/etc/bind/named.conf.options` seperti pada ***MALANG***
* Edit pada Mojokerto file `/etc/bind/named.conf.local` menjadi
  ```
  zone "gunung.semerut13.pw" {
  	type master;
	file "/etc/bind/delegasi/gunung.semerut13.pw";
	allow-transfer { any; };
  };
  ```
* Kemudian buat direktori *delegasi* dan copy db.local ke file delegasi
  ```
  mkdir /etc/bind/delegasi
  cp /etc/bind/db.local /etc/bind/delegasi/gunung.semerut13.pw
  ```
* Edit file `/etc/bind/delegasi/gunung.semerut13.pw`

  ![](/images/6-3.png)

* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Kemudian cek di client **GRESIK** dengan perintah `ping gunung.semerut13.pw`

  ![](/images/6-4.png)


*__SOAL No. 7__*
---

### Subdomain dengan nama http://naik.gunung.semerut13.pw, domain ini diarahkan ke IP Server PROBOLINGGO. Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server.

* Edit file pada Mojokerto `/etc/bind/delegasi/gunung.semerut13.pw` lalu tambahkan seperti dibawah :
  ```
  @	          IN	  NS	      gunung.semerut13.pw.
  @	          IN	  A           10.151.77.156	; IP PROBOLINGGO
  naik		  IN	  A	      10.151.77.156	; IP PROBOLINGGO
  ```

  ![](/images/7-1.png)

* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Kemudian cek di client **GRESIK** dengan perintah `ping gunung.semerut13.pw`

  ![](/images/7-2.png)

*__SOAL No. 8__*
---

### Domain http://semerut13.pw memiliki DocumentRoot pada /var/www/semerut13.pw. Awalnya web dapat diakses menggunakan alamat http://semerut13.pw/index.php/home . Karena dirasa alamat urlnya kurang bagus, maka

* Pindah ke directory `/etc/apache2/sites-available`
* Copy file default menjadi file **semerut13.pw** Jangan lupa untuk menambahkan `.conf` jika apache2 versi 2.4.x
* Buka file **semerut13.pw** , tambahkan :
```
 ServerName semerut13.pw
 ServerAlias semerut13.pw
```
* Ubah DocumentRoot menjadi `/var/www/semerut13.pw`

 ![](/images/8-1.png)
 
* Aktifkan konfigurasi **semerut13.pw** , Gunakan perintah `a2ensite jarkom2020.com`
* Restart apache dengan perintah `service apache2 restart`
* Pindah ke directory `/var/www`
* Download file dengan perintah `wget 10.151.36.202/semeru.pw.zip`
* Unzip , lalu rename menjadi **semerut13.pw**
* Buka browser dan Akses **semeru13.pw/index.php/home**

 ![](/images/8-2.jpg)
 
*__SOAL No. 9__*
---

### Diaktifkan mod rewrite agar urlnya menjadi http://semerut13.pw/home.

* Aktifkan perintah `a2enmod rewrite` untuk mengaktifkan module rewrite.
* Restart apache dengan perintah `service apache2 restart`
* Pindah ke directory `/var/www/semerut13.pw` dan buat file `.htaccess` dengan isi file
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```
* Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `semerut13.pw` dan tambahkan
```
 <Directory /var/www/jarkom2020.com>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
 </Directory>
```
* Restart apache dengan `service apache2 restart`
* Buka browser dan akses semerut13.pw/home

*__SOAL No. 10__*
---

### Web http://penanjakan.semerut13.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semerut13.pw dan memiliki struktur folder sebagai berikut:
```
/var/www/penanjakan.semerut13.pw
 	/public/javascripts
 	/public/css
 	/public/images
	/errors
```
* Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `penanjakan.semerut13.pw` dan tambahkan :
```
 <Directory /var/www/jarkom2020.com/assets>
     Options +Indexes
 </Directory>
```
*jangan lupa untuk menyimpan perubahan tersebut agar directory download menampilkan isi directory-nya.*
* dan isi sesuai dengan soal
* Restart apache dengan perintah `service apache2 restart`
* Buka browser dan akses http://penanjakan.semerut13.pw/ **(aku bingungn nulisnya njir desss sama nomer 11 jugakk)**

*__SOAL No. 11__*
---

### Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.

*__SOAL No. 12__*
---

### Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.

* Pindah ke directory `etc/apache2/sites-available`
* Kemudian buka file `penanjakan.semerut13.pw`
* Tambahkan `ErrorDocument 404 /errors/404.html`
* Gunakan perintah `service apache2 restart` untuk merestart apache


*__SOAL No. 13__*
---

### Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semerut13.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semerut13.pw/js. Untuk web http://gunung.semerut13.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai.

