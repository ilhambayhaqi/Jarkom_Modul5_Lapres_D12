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
## D. Melakukan pembagian IP dengan DHCP SERVER dan RELAY.

Untuk pengaturan network interface tiap UML dibuat sebagai berikut.

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

