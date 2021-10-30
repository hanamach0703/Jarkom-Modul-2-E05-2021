# Jarkom-Modul-2-E05-2021

Nama Kelompok:
1. Muhammad Rafki Mardi 05111940000054
2. Hana Machmudah 05111940000072
3. Achmad Fadillah  05111940000155

## No. 1
### EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

## Jawaban
Membuat topologi seperti ini:
![image](https://user-images.githubusercontent.com/66562311/139523501-1c54c751-300f-4a77-8f71-a5d861cd99e2.png)

Configurasi node:
FOOSHA
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.202.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.202.2.1
	netmask 255.255.255.0
```
Loguetown
```
auto eth0
iface eth0 inet static
	address 192.202.1.2
	netmask 255.255.255.0
	gateway 192.202.1.1
```
Alabasta
```
auto eth0
iface eth0 inet static
	address 192.202.1.3
	netmask 255.255.255.0
	gateway 192.202.1.1
```
EniesLobby
```
auto eth0
iface eth0 inet static
	address 192.202.2.2
	netmask 255.255.255.0
	gateway 192.202.2.1
```
Water7
```
auto eth0
iface eth0 inet static
	address 192.202.2.3
	netmask 255.255.255.0
	gateway 192.202.2.1
```
Skypie
```
auto eth0
iface eth0 inet static
	address 192.202.2.4
	netmask 255.255.255.0
	gateway 192.202.2.1
```

Command pada FOOSHA 
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16
```
dan selanjutnya command pada FOOSHA
```
cat /etc/resolv.conf
``` 
kemudian command pada semua node Longuetown, Alabasta, EniesLobby, Water7, Skypie
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Coba dengan melakukan `ping -c 4 google.com` pada setiap node.
![image](https://user-images.githubusercontent.com/66562311/139523707-e52c9f4b-3941-4536-ac78-de94e84b8984.png)

## No. 2
### Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.

## Jawaban
### Pada node EneisLobby
- Menulis command untuk update package lists
  ```
  apt-get update
  ```
- Menulis command untuk install aplikasi bind9 pada EniesLobby
  ```
  apt-get install bind9 -y
  ```
- Edit file 
  ```
  vim /etc/bind/named.conf.local
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523792-e25a6105-cc1f-4bce-92eb-3c3133ecf1ab.png)

- Buat folder kaizoku dengan 
  ```
  mkdir /etc/bind/kaizoku
  ```
- Copy kan db.local ke dalam folder kaizoku dan ubah nama nya menjadi `franky.E05.com`
  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/franky.E05.com
  ```
- Edit file 
  ```
  vim /etc/bind/kaizoku/franky.E05.com
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523870-41e12988-5f22-4cfd-9382-817ca103f05a.png)

- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
    
### Pada node Loguetown
- Edit file 
  ```
  vim /etc/resolv.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523950-42777481-2fb7-410a-b56a-ec7aed8ce9b3.png)

- Melakukan testing dengan cara ping `franky.E05.com` dan `ping www.franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139524101-ed350b72-e44f-4745-a951-0e4584f50b2f.png)


## No. 3
### Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.

## Jawaban
### Pada EniesLobby
- Edit file 
  ```
  vim /etc/bind/kaizoku/franky.E05.com
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523888-e03f9f67-0fcc-41c8-8151-bb46442e07d4.png)

- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
### Pada node Loguetown
- Edit file 
  ```
  vim /etc/resolv.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523981-8fcb0fe1-2a1a-4426-b591-219467676e19.png)

- Melakukan testing dengan cara ping `super.franky.E05.com` dan `ping www.super.franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139524140-aa033558-f316-480a-9159-598f5e685328.png)

## No. 4
### Buat juga reverse domain untuk domain utama.

## Jawaban
### Pada EniesLobby
- Edit file 
  ```
  /etc/bind/named.conf.local 
  ```
  seperti pada gambar berikut:
  ![image](https://user-images.githubusercontent.com/66562311/139524734-5e443e53-5a16-4574-832d-ab051b6a3c72.png)
  
- Edit file 
  ```
  /etc/bind/kaizoku/2.202.192.in-addr.arpa 
  ```
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
### Pada Loguetown
  Melakukan testing konfigurasi dengan perintah host -t PTR 192.202.2.2.

## No. 5
### Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

## Jawaban

## No. 6
### Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

## Jawaban

## No. 7
### Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.


## No. 8
### Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.

## Jawaban

## No. 9
### Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.

## Jawaban

## No. 10
### Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.

## Jawaban

## No. 11
### Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

## Jawaban

## No. 12
### Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

## Jawaban

## No. 13
### Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js. 

## Jawaban

## No. 14
### Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.

## Jawaban

## No. 15
### dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy.

## Jawaban

## No. 16
### Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com.


## No. 17
### Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!
