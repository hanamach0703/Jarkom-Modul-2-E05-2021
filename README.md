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
### EneisLobby
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
    
### Loguetown
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
### EniesLobby
- Edit file 
  ```
  vim /etc/bind/kaizoku/franky.E05.com
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139523888-e03f9f67-0fcc-41c8-8151-bb46442e07d4.png)

- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
### Loguetown
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
### EniesLobby
- Edit file 
  ```
  /etc/bind/named.conf.local 
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139525287-e6d48aa7-c6d6-477b-bd97-dd2e6e9844c7.png)
  
- Edit file 
  ```
  /etc/bind/kaizoku/2.202.192.in-addr.arpa 
  ```
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
### Loguetown
- Melakukan testing konfigurasi dengan perintah host -t PTR 192.202.2.2.
  ![image](https://user-images.githubusercontent.com/66562311/139525236-a8035a3b-1aad-48bc-963e-a7fa7af2f6bb.png)


## No. 5
### Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

## Jawaban
### EniesLobby
- Edit file 
  ```
  /etc/bind/named.conf.local
  ```
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
  
### water7 
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
  /etc/bind/named.conf.local 
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139525534-a3668e1c-2675-43cb-95e7-bbe8a4d073c8.png)

- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
  
### Loguetown
- Edit file 
  ```
  vim /etc/resolv.conf
  ``` 
  ![image](https://user-images.githubusercontent.com/66562311/139525566-c2d46a11-c594-4b9c-991c-80c222b83f6f.png)
  
- Melakukan testing `ping -c 4 franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139525695-5a274437-a5fe-44d1-a3bd-171d9f7e3b07.png)
  ![image](https://user-images.githubusercontent.com/66562311/139525727-67d85604-e96f-4ced-a574-50d7623ffd98.png)  


## No. 6
### Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

## Jawaban
### EniesLobby
- Edit file 
  ```
  /etc/bind/kaizoku/franky.E05.com 
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139526023-745a779a-816f-4775-a3e5-cc2ee4a000aa.png)
- Edit file 
  ```
  /etc/bind/named.conf.options
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139526829-1ec1747e-28a0-49e2-a112-6051ee347a2e.png)
- Edit file 
  ```
  /etc/bind/named.conf.local
  ```
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```

### Water7
- Edit file 
  ```
  /etc/bind/named.conf.options 
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139526837-c481b324-4274-4912-9462-f4eb2b5c6157.png)
- Edit file 
  ```
  /etc/bind/named.conf.local
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139526862-8bed543f-5629-4ff1-831b-b736ce24e5a6.png)
- Buat folder sunnygo di dalam /etc/bind.
  ```
  mkdir /etc/bind/sunnygo
  ```
- Copykan file db.local pada path /etc/bind ke dalam folder sunnygo yang baru saja dibuat dan ubah namanya menjadi mecha.franky.E05.com.
  ```
  cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.E05.com
  ```
- Edit file 
  ```
  /etc/bind/sunnygo/mecha.franky.E05.com
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139526877-2f9d0f85-8efa-4c65-82a9-ad46b8a4d500.png)
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
  
### Loguetown
- Melakukan testing `ping -c 4 mecha.franky.E05.com` dan `ping -c 4 www.mecha.franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139526901-3f72eaa5-29a9-4fe2-a861-833bcff70bea.png)


## No. 7
### Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.

## Jawaban
### Water7
- Edit file 
  ```
  /etc/bind/sunnygo/mecha.franky.E05.com
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139527062-f5c64b21-5a03-4bda-bc93-6e0cb564e66b.png)
- Restart bind9 dengan 
  ```
  service bind9 restart
  ```
  
### Loguetown
- Melakukan testing `ping -c 4 general.mecha.franky.E05.com` dan `ping -c 4 www.general.mecha.franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139527100-2f8e5617-cc66-498a-a4a5-cf0033399043.png)


## No. 8
### Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.

## Jawaban
### EniesLobby
- Edit File
  ![image](https://user-images.githubusercontent.com/66562311/139527503-bbddfde5-6586-40b0-87f2-9c62a9d22acf.png)	

### Skypie
- Install aplikasi apache, PHP, dan libapache2-mod-php7.0.
  ```
  apt-get install apache2 -y
  apt-get install php -y
  apt-get install libapache2-mod-php7.0 -y
  ```
- Pindah ke directory 
  ```
  /etc/apache2/sites-available
  ```
- Copy file `000-default.conf` menjadi file `franky.E05.com.conf`
- Edit file 
  ```
  franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139527608-756d777c-0b90-4517-b9e0-6652498219fe.png)
  ![image](https://user-images.githubusercontent.com/66562311/139527642-7f036a30-57b9-4d40-8fdd-df38f04df4ef.png)

- Aktifkan konfigurasi franky.E05.com.
  ```
  a2ensite franky.E05.com
  ```
- Restart apache.
  ```
  service apache2 restart
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139527444-58342d67-c941-4ef2-a1db-6fa77f3d4fb1.png)
- Pindah ke directory `/var/www`.
- Download file zip menggunakan wget.
  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip
  ```
- Melakukan unzip.
  ```
  unzip franky.zip
  ```
- Rename folder franky menjadi franky.E05.com dan terdapat isi file seperti pada gambar berikut:

### Loguetown
- Install aplikasi lynx.
  ```
  apt-get install lynx -y
  ```
- Buka `www.franky.E05.com` menggunakan `lynx`.
  ![image](https://user-images.githubusercontent.com/66562311/139527668-de61b7bf-8e9d-447c-a905-10c702696fe1.png)


## No. 9
### Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.

## Jawaban

## No. 10
### Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.

## Jawaban
### Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `super.franky.E07.com.conf`.
- Edit file 
  ```
  super.franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532234-ca62e369-34e5-4887-a61c-b03768142e67.png)

- Aktifkan konfigurasi.
  ```
  a2ensite super.franky.E05.com
  ```
- Restart apache.
  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`
- Download file zip.
  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip
  ```
- Melakukan unzip.
  ```
  unzip super.franky.zip
  ```
- Rename folder `super.franky` menjadi `super.franky.E05.com` dan terdapat isi file seperti pada gambar berikut:
  ![image](https://user-images.githubusercontent.com/66562311/139532265-c8b12743-49c8-4a84-98c1-5d134fa03477.png)
  
## No. 11
### Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

## Jawaban
### Pada Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file 
  ```
  super.franky.E05.com.conf 
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532421-6c849075-59a3-4897-921d-b3278e8a3d20.png)
- Restart apache.
  ```
  service apache2 restart
  ```
  
### Loguetown
- Membuka `www.super.franky.E05.com/public` menggunakan `lynx`.
![image](https://user-images.githubusercontent.com/66562311/139532439-fe77e402-9401-4d3f-a46b-532ffbcc5f44.png)


## No. 12
### Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

## Jawaban
### Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file 
  ```
  super.franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532561-1c1410fa-1ba9-4c89-af38-ee6ea5a604c2.png)
- Restart apache.
  ```
  service apache2 restart
  ```

### Pada Loguetown
- Buka `www.super.franky.E05.com/public` menggunakan lynx.
![image](https://user-images.githubusercontent.com/66562311/139532639-f72b853f-8946-434a-b8f8-246e3c939993.png)
https://github.com/hanamach0703/Jarkom-Modul-2-E05-2021/blob/main/picture/nomer%2012%20pt%205.mkv?raw=true

## No. 13
### Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js. 

## Jawaban
### Skypie
- Pindah ke directory /etc/apache2/sites-available.
- Edit file 
  ```
  super.franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532743-b5611c95-2034-4304-b1d1-173730c6f26e.png)
  ![image](https://user-images.githubusercontent.com/66562311/139532755-0a6fb851-523c-4d17-8d5e-66802c326666.png)

- Restart apache.
  ```
  service apache2 restart
  ```

### Pada Loguetown
- Buka `www.super.franky.E05.com/js` menggunakan lynx.
![image](https://user-images.githubusercontent.com/66562311/139532767-07fd7cf2-3fbd-4182-8cdb-ef5aac16ec08.png)


## No. 14
### Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.

## Jawaban
### Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `general.mecha.franky.E05.com.conf`.
- Edit file 
  ```
  general.mecha.franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532970-bea7daf6-cd6c-4000-bb04-4e98c62690e2.png)

- Edit file, untuk meaktifkan port 15000 dan port 15500
  ```
  /etc/apache2/ports.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139532988-91ff58c5-fde4-42dc-8558-b8b4bb9dacfa.png)

- Aktifkan konfigurasi
  ```
  a2ensite general.mecha.franky.E05.com
  ```
- Restart apache.
  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Download file zip menggunakan `wget`.
  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip
  ```
- Lakukan unzip.
  ```
  unzip general.mecha.franky.zip
  ```
- Rename folder g`eneral.mecha.franky` menjadi `general.mecha.franky.E05.com`

### Pada Loguetown
- Buka `www.general.mecha.franky.E05.com` menggunakan `lynx`.
  ![image](https://user-images.githubusercontent.com/66562311/139532998-8dfacd8c-be85-4ad7-9608-e19a6ca3394f.png)

- Buka `www.general.mecha.franky.E05.com:15000` menggunakan `lynx`.
  ![image](https://user-images.githubusercontent.com/66562311/139533014-03301dd9-d916-4b71-a094-b889759fb86d.png)

- Buka `www.general.mecha.franky.E05.com:15500` menggunakan `lynx`.
 ![image](https://user-images.githubusercontent.com/66562311/139533041-76b8d6df-9b8c-47aa-9c29-45197a564be6.png)


## No. 15
### dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy.

## Jawaban
### Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file 
  ```
  general.mecha.franky.E05.com.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139533308-257a3675-10cc-4969-ab26-f50de8a0b06e.png)

- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username luffy. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi onepiece.
  ```
  htpasswd -c /etc/apache2/.htpasswd luffy
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139533345-daeb46ce-f3d1-4d2b-8975-050f215acbc3.png)

- Restart apache.
  ```
  service apache2 restart
  ```
  
### Pada Loguetown
- Buka `www.general.mecha.franky.E05.com:15000` menggunakan `lynx`.
  https://github.com/hanamach0703/Jarkom-Modul-2-E05-2021/blob/main/picture/nomer%2015%20pt%203.mkv?raw=true

## No. 16
### Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com.

## Jawaban
### Skypie
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file
  ```
  000-default.conf
  ```
  ![image](https://user-images.githubusercontent.com/66562311/139533449-983c9ec8-c6bb-4df5-82a6-28ad9e1af6a9.png)

- Restart apache.
  ```
  service apache2 restart
  ```

### Pada Loguetown
- Buka 192.181.2.4 menggunakan lynx.
https://github.com/hanamach0703/Jarkom-Modul-2-E05-2021/blob/main/picture/nomer%2016%20pt%204.mkv?raw=true

## No. 17
### Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!

## Jawaban
### Pada Skypie
- Mengaktifkan module rewrite dengan menjalankan perintah 
  ```
  a2enmod rewrite 
  ```
- Restart apache
  ```
  service apache2 restart
  ```
- Menambahkan file baru `.htaccess` pada folder `/var/www/super.franky.E05.com`
  ![image](https://user-images.githubusercontent.com/66562311/139534595-6f7529fb-e5f1-41b3-a6a8-408bd42ea15c.png)

- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `super.franky.E05.com.conf`.
  ![image](https://user-images.githubusercontent.com/66562311/139534583-306a5e3a-634d-4bb3-9a1c-8abf9008b220.png)

- Restart apache.
  ```
  service apache2 restart
  ```
### Pada Loguetown
- Buka `www.super.franky.E05.com/public/images/franky.png` menggunakan `lynx`.
- Buka `www.super.franky.E05.com/public/images/eyeoffranky.jpg` menggunakan `lynx`.
- Buka `www.super.franky.E05.com/public/images/background-frank.jpg` menggunakan `lynx`.


