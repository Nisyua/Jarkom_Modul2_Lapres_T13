# Jarkom_Modul2_Lapres_T13
---

* Anis Saidatur Rochma 05311840000002
* Desya Ananda Puspita Dewi 05311840000046

### Requirements :
---
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
  
* No. 4
* No. 5
* No. 6
* No. 7
* No. 8
* No. 9
* No. 10
* No. 11
* No. 12
* No. 13
* No. 14
* No. 15
* No. 16
* No. 17
