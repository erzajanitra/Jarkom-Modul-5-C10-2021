# Jarkom-Modul-5-C10-2021

## Anggota Kelompok C10
| Nama | NRP |
| ------------- | ------------- |
| Christian Bennett Robin | 05111940000078  |
| Erza Janitradevi Nadine  | 05111940000153  |
| Akmal Zaki Asmara  | 05111940000154  |

## Soal dan Pembahasan
### A
### B
### C
### D
### No 1
### No 2
### No 3
### No 4
Soal : 
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Jawaban : 
Ditambahkan barisan kode ini pada Doriki:
```
iptables -A INPUT -s 10.19.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.19.4.0/23  -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.19.4.0/23 -j REJECT
iptables -A INPUT -s 10.19.0.128/25 -j REJECT
```

Barisan kode ini digunakan untuk memberi akses ke Doriki, namun hanya untuk jam 07:00 sampai 15:00 pada hari Senin sampai Kamis, selain dari waktu itu akan di reject. `10.19.0.128/25` disini merupakan NID dari subnet Blueno dan `10.19.4.0/23` merupakan NID dari subnet Cipher.

Untuk mengetes, bisa menggunakan `date -s “8 nov 2021 10:00:00”` agar waktunya sesuai dan akses tidak dibatasi, lalu bisa coba ping ke google dari salah satu kliennya:

![4](images/4.png)

### No 5
Soal : 
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
Selain itu di reject

Jawaban : 
Sama seperti nomor 4, ditambahkan barisan kode pada Doriki:

```
iptables -A INPUT -s 10.19.2.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT 
iptables -A INPUT -s 10.19.2.0/23 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.19.2.0/23 -j REJECT
iptables -A INPUT -s 10.19.1.0/24 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 10.19.1.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.19.1.0/24 -j REJECT
```

Seperti nomor 4, akses akan diberikan, namun hanya dari jam 00:00 sampai jam 06:59. `10.19.2.0/23` disini merupakan NID dari subnet Elena dan `10.19.1.0/24` merupakan NID dari subnet Fukurou.

Untuk mengetes, bisa menggunakan `date -s “8 nov 2021 06:00:00”` agar waktunya sesuai dan akses tidak dibatasi, lalu bisa coba ping ke google dari salah satu kliennya:

![5](images/5.png)

### No 6
Soal : 
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

Jawaban : 
Pada Guanhao, ditambahkan kode ini:

```
iptables -A PREROUTING -t nat -p tcp -d 10.19.0.8/29 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination  10.19.0.18:80
iptables -A PREROUTING -t nat -p tcp -d 10.19.0.8/29 -j DNAT --to-destination 10.19.0.19:80
iptables -t nat -A POSTROUTING -p tcp -d 10.19.0.18 -j SNAT --to-source 10.19.0.8:80
iptables -t nat -A POSTROUTING -p tcp -d 10.19.0.19 -j SNAT --to-source 10.19.0.8:80
```

Disini Guanhao akan diatur agar setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

`10.19.0.8/29` merupakan NID dari subnet DNS Server yaitu subnet yang terdapat Doriki, Jipangu, dan Water7. Disini digunakan` --mode nth --every 2 --packet 0` untuk mengatur agar terdistribusi ke 2 server, yaitu yang pertama untuk server dengan IP `10.19.0.18:80` (dengan port 80), yaitu Jorge, dan IP `10.19.0.19:80`, yaitu Maingate.

Untuk mengetesnya akan dilakukan di client Elena dengan menggunakan netcat. Jangan lupa untuk menginstall netcat dulu di Elena, Jorge, dan Maingate dengan cara:

```
apt-get update
apt-get install netcat -y
```

Setelah itu, ketik ini dalam terminal di Jorge dan Maingate:

```
nc -l -p 80
```

Ketik ini dalam terminal di Elena:

```
nc 10.19.0.8 80
```

Lalu ketikkan apapun dalam terminal tersebut, maka hal yang diketikkan akan muncul di salah satu server Jorge atau Maingate:

Di Elena:

![6a](images/6a.png)

Maka ketikkan tersebut akan muncul di Jorge:

![6b](images/6b.png)

Setelah itu untuk mengetes apakah dia terdistribusi ke server yang satunya, coba tutup terminal Elena dan di run lagi command yang tadi dan ketikkan apapun lagi:

![6c](images/6c.png)

Maka ketikkan tersebut akan muncul di server satunya, yaitu Maingate:

![6d](images/6d.png)

