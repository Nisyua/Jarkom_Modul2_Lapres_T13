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
	file "/etc/bind/jarkom/jarkom2020.com";
};
  ```
  * Buat folder  di dalam `/etc/bind/jarkom`
  * Copykan file `db.local` pada path `/etc/bind` ke dalam folder jarkom yang baru saja dibuat :
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/semerut13.pw
  ```
  * Buka dan edit file semerub12.pw dengan perintah : 
  ```
  nano /etc/bind/jarkom/semerub13.pw
  ```
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
  
*__SOAL No. 2__*
---
### alias http://www.semerub12.pw

  * menambahkan `cname` dengan menuliskan `www`
  ```
  www   IN   CNAME   semerut13.pw
  ```
  * Kemudian restart bind9 dengan perintah

```
service bind9 restart
```
* Lalu cek di client **Gresik** dengan perintah `ping www.semerut13.pw`

*__SOAL No. 3__*
---
### subdomain http://penanjakan.semerub12.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO serta dibuatkan

* Edit file pada Malang `/etc/bind/jarkom/semerut13.pw` lalu tambahkan seperti dibawah :
```
@	            IN	  A	      10.151.83.108	; IP PROBOLINGGO
www	          IN	  CNAME	  semerub12.pw.
penanajakan	  IN	  A	      10.151.83.108	; IP PROBOLINGGO
```
* Edit `nano /etc/bind/named.conf.local`
* Lalu ***Restart*** bind9 dengan perintah `service bind9 restart`
* Pergi ke **Gresik** dan lakukan testing dengan perintah `ping penanjakan.semerut13.pw`

*__SOAL No. 4__*
---
### reverse domain untuk domain utama. Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan

  * Edit file `/etc/bind/named.conf.local` pada ***MALANG***
  ```
  nano /etc/bind/named.conf.local
  ```
  * Lalu tambahkan konfigurasi berikut ke dalam file `named.conf.local`
  ```
  zone "71.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/77.151.10.in-addr.arpa";
};
  ```
  * Copykan file db.local pada path /etc/bind ke dalam folder jarkom dengan perintah
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/77.151.10.in-addr.arpa
  ```
  * Kemudian edit file dengan `nano /etc/bind/jarkom/77.151.10.in-addr.arpa`
  * Restart bind9 dengan `service bind9 restart`
  
*__SOAL No. 5__*
---

### DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website. Selain website utama Bibah juga meminta dibuatkan

*__SOAL No. 6__*
---
### Subdomain dengan alamat http://gunung.semerub12.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO. Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan


*__SOAL No. 7__*
---

### Subdomain dengan nama http://naik.gunung.semerub12.pw, domain ini diarahkan ke IP Server PROBOLINGGO. Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server.

*__SOAL No. 8__*
---

### Domain http://semerub12.pw memiliki DocumentRoot pada /var/www/semerub12.pw. Awalnya web dapat diakses menggunakan alamat http://semerub12.pw/index.php/home . Karena dirasa alamat urlnya kurang bagus, maka

*__SOAL No. 9__*
---

### Diaktifkan mod rewrite agar urlnya menjadi http://semerub12.pw/home.

*__SOAL No. 10__*
---

### Web http://penanjakan.semerub12.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semerub12.pw dan memiliki struktur folder sebagai berikut:
```
/var/www/penanjakan.semerub12.pw
 	/public/javascripts
 	/public/css
 	/public/images
	/errors
```

*__SOAL No. 11__*
---

### Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.

*__SOAL No. 12__*
---

### Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.

*__SOAL No. 13__*
---

### Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semerub12.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semerub12.pw/js. Untuk web http://gunung.semerub12.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai.

