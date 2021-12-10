# Jarkom-Modul-5-C10-2021

## Subnetting menggunakan VLSM
### Pembagian Subnet
![vlsm fix-topologi](https://user-images.githubusercontent.com/75319371/145598429-7fb93ad5-c8d5-440d-9361-369d0641605f.jpg)
| Subnet | Jumlah IP | Netmask |	
|:------:|:---------:|:-------:|	
|   A1   |    3   |   /29   |	
|   A2   |    101    |   /25   |	
|   A3   |     701     |   /22   |	
|   A4   |      2  |   /30   |	
|   A5   |     2    |   /30   |	
|   A6   |    301   |   /23   |	
|   A7   |     201     |   /24   |	
|   A8   |    3    |   /29  |	
|  Total   |    1314    |   /21  |	
### Pembagian IP
![vlsm fix-tree](https://user-images.githubusercontent.com/75319371/145599817-c2b24ace-6b77-4cdb-8155-317e91344737.jpg)

|  A1 | Network ID        | 10.19.0.8     |	
|:---:|-------------------|-----------------|	
|     | Netmask           | 255.255.255.248   |	
|     | Broadcast Address | 10.19.0.15    |	
|  A2 | Network ID        | 10.19.0.128     |	
|     | Netmask           | 255.255.255.128 |	
|     | Broadcast Address | 10.19.0.255     |	
|  A3 | Network ID        | 10.19.4.0      |	
|     | Netmask           | 255.255.252.0|	
|     | Broadcast Address | 10.19.7.255     |	
|  A4 | Network ID        | 10.19.4.0       |	
|     | Netmask           | 255.255.252.0   |	
|     | Broadcast Address | 10.19.7.255     |	
|  A5 | Network ID        | 10.19.0.0      |	
|     | Netmask           | 255.255.255.252 |	
|     | Broadcast Address | 10.19.0.3       |	
|  A6 | Network ID        | 10.19.2.0       |	
|     | Netmask           | 255.255.254.0   |	
|     | Broadcast Address | 10.19.3.255    |	
|  A7 | Network ID        | 10.19.1.0       |	
|     | Netmask           | 255.255.255.0 |	
|     | Broadcast Address | 10.19.1.255      |	
|  A8 | Network ID        | 10.19.0.16   |	
|     | Netmask           | 255.255.255.248  |	
|     | Broadcast Address | 10.19.0.23    |
## Topologi
![topologi gns](https://user-images.githubusercontent.com/75319371/145600493-3996ee27-3768-4dce-bb31-0f34e367afa8.JPG)

## Routing
- Foosha
```
route add -net 10.19.0.8 netmask 255.255.255.248 gw 10.19.0.2
route add -net 10.19.0.128 netmask 255.255.255.128 gw 10.19.0.2
route add -net 10.19.4.0 netmask 255.255.252.0 gw 10.19.0.2
route add -net 10.19.0.16 netmask 255.255.255.248 gw 10.19.0.6
route add -net 10.19.2.0 netmask 255.255.254.0 gw 10.19.0.6
route add -net 10.19.1.0 netmask 255.255.255.0 gw 10.19.0.6
```
- Water7
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.19.0.1
```
- Guanhao
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.19.0.5
```
