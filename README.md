# Jarkom_Modul5_Lapres_D12
- Muhammad Ilham Bayhaqi - 05111840000069
- Clever Dicki Marpaung - 05111840000116

## A. Membuat Topologi

![Capture](https://user-images.githubusercontent.com/57692117/103269615-c7e75900-49e8-11eb-9eba-43d7c726af8b.JPG)

Pada problem ini dibuatkan file topologi.sh sebagai berikut.

![image](https://user-images.githubusercontent.com/57692117/103269970-8b682d00-49e9-11eb-94ad-78b8f7a3369c.png)

## B. Melakukan Pembagian IP
Pada modul ini, kami menggunakan pembagian IP dengan VSLM dimana untuk tabel pembagian IPnya sebagai berikut.

![messageImage_1608734255098](https://user-images.githubusercontent.com/57692117/103269431-6f17c080-49e8-11eb-9df0-e589b96d3779.jpg)

Konfigurasi untuk interface tiap UML(tanpa DHCP) sebagai berikut.

- **SURABAYA**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.78.54
netmask 255.255.255.252
gateway 10.151.78.53

auto eth1
iface eth1 inet static
address 192.168.2.1
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.2.5
netmask 255.255.255.252
```
- **BATU**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.2
netmask 255.255.255.252

auto eth1
iface eth1 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 10.151.79.105
netmask 255.255.255.248
```

- **KEDIRI**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.6
netmask 255.255.255.252

auto eth1
iface eth1 inet static
address 192.168.2.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.0
```

- **MALANG**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.79.106
netmask 255.255.255.248
gateway 10.151.79.105
```

- **MOJOKERTO**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.79.107
netmask 255.255.255.248
gateway 10.151.79.105
```
## C. Melakukan Routing

Untuk routing dilakukan pada UML **SURABAYA**, **BATU**, dan **KEDIRI** sebagai berikut.

- **SURABAYA**
```
route add -net 192.168.0.0 netmask 255.255.255.0 gw 192.168.2.6
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.2.2
route add -net 192.168.2.8 netmask 255.255.255.248 gw 192.168.2.6
route add -net 10.151.79.104 netmask 255.255.255.248 gw 192.168.2.2
```

- **BATU**
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.2.1
```

- **KEDIRI**
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.2.5
```
## D. Melakukan Konfigurasi DHCP.

Pada file dhcpd.conf DHCP Server di **Mojokerto**, dibuat pembagian IP client.

![image](https://user-images.githubusercontent.com/57692117/103285931-f4fc3180-4a11-11eb-9217-80b09178a9eb.png)

![image](https://user-images.githubusercontent.com/57692117/103285986-11986980-4a12-11eb-90e5-f1f06688fb9c.png)

![image](https://user-images.githubusercontent.com/57692117/103286029-296fed80-4a12-11eb-9125-35f77e1384d9.png)

Pada file isc-dhcp-server di **Mojokerko** dilakukan penyesuaian interfacenya.

![image](https://user-images.githubusercontent.com/57692117/103286242-76ec5a80-4a12-11eb-9eed-2e7556f3e2a8.png)

Pada tiap-tiap router diinstall DHCP Relay, dan pada file isc-dhcp-relay dilakukan konfigurasi sebagai berikut.

![image](https://user-images.githubusercontent.com/57692117/103286503-d5193d80-4a12-11eb-9baa-92895aa504c5.png)

![image](https://user-images.githubusercontent.com/57692117/103286535-e6624a00-4a12-11eb-9166-ea6087733928.png)

![image](https://user-images.githubusercontent.com/57692117/103286569-fc700a80-4a12-11eb-8664-57aec7ff5df7.png)


Untuk pengaturan network interface tiap UML yang menggunakan DHCP dibuat sebagai berikut.

- **SIDOARJO**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

- **GRESIK**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

- **PROBOLINGGO**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

- **MADIUN**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

## Soal1

Dikarenakan pada soal tidak diperbolehkan menggunakan MASQUERADE, maka kita menggunakan konfigurasi iptables sebagai berikut.

- **SURABAYA**
```
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 10.151.78.54 
```

Semua paket yang menuju keluar melalui eth0 akan dirubah source IP address nya menjadi 10.151.78.54.

## Soal2 & Soal7

Diminta untuk drop semua akses SSH dari luar Topologi yang menuju DHCP dan DNS SERVER (MOJOKERTO & MALANG)
Diminta untuk melakukan konfigurasi iptables pada UML SURABAYA.
Untuk Soal7 diminta untuk melakukan log pada setiap paket yang didrop

- **SURABAYA**
```
iptables -N LOGGING
iptables -A FORWARD -p tcp --dport 22 -d 10.151.79.104/29 -i eth0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: "
iptables -A LOGGING -j DROP
```

- Sebelum diberlakukan iptables

![image](https://user-images.githubusercontent.com/57692117/103285638-4821b480-4a11-11eb-9feb-891e54ea3383.png)

- Setelah diberlakukan iptables

![image](https://user-images.githubusercontent.com/57692117/103285686-6c7d9100-4a11-11eb-95ad-7aca066415cd.png)

Dibuatkan variable baru dengan nama ```LOGGING```, dimana nantinya akan digunakan dalam pembuatan log. Karena IP Address yang dituju
MOJOKERTO dan MALANG yang jika kita lihat dari Topologi bahwa mereka melewati SURABAYA, maka di SURABAYA digunakan chain FORWARD.
Semua paket yang masuk melalui eth0 dengan menggunakan protokol tcp dan menuju port 22 serta menuju subnet 10.151.79.104/29, akan
didrop dan log nya akan ditampilkan pada UML SURABAYA.

## Soal3 & Soal7

Diminta untuk membatasi akses koneksi ke DHCP dan DNS Server (MOJOKERTO dan MALANG) sebanyak tiga koneksi dengan protokol ICMP, selebihnya didrop

- **MOJOKERTO**
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: "
iptables -A LOGGING -j DROP
```

- **MALANG**
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: "
iptables -A LOGGING -j DROP
```

Sama seperti penjelasan sebelumnya mengenai LOGGING. Disini kita buatkan konfigurasi dimana setiap paket yang masuk dengan menggunakan
protokol ICMP akan dibatasi koneksinya maksimal sebanyak tiga koneksi, selebihnya akan di drop.

## Soal4 & Soal5

Diminta untuk membatasi akses ke MALANG yang berasal dari subnet SIDOARJO dan GRESIK pada jam tertentu.

- **MALANG**
```
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT #4
iptables -A INPUT -s 192.168.0.0/24 -m time --timestart 17:00 --timestop 23:59 -j ACCEPT #5
iptables -A INPUT -s 192.168.0.0/24 -m time --timestart 00:00 --timestop 07:00 -j ACCEPT #5
iptables -A INPUT -j REJECT #4
```

Terdapat dua source IP Address yang akan dilimitasi akses waktunya, dimana keduanya itu ialah IP Address SIDOARJO dan GRESIK.
Untuk subnet SIDOARJO dapat diakses pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat, dan subnet GRESIK pada pukul 17.00
sampai 07.00 setiap harinya.

## Soal 6

Membuat Konfigurasi Firewall pada **SURABAYA** untuk melakukan load balancer ketika client mengakses IP Malang akan dialihkan menuju Probolinggo atau Madiun
- **SURABAYA**

```
iptables -A PREROUTING -t nat -p tcp -d 10.151.79.106 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.2.10:80
iptables -A PREROUTING -t nat -p tcp -d 10.151.79.106 -j DNAT --to-destination 192.168.2.11:80
```
![image](https://user-images.githubusercontent.com/57692117/103285450-c29e0480-4a10-11eb-8c25-214c9b4526b7.png)

