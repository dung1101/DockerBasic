# Docker network  

|Driver|Mô tả|
|------|-----|
|`none`|Các container thiết lập network này sẽ không được cấu hình mạng.|
|`bridge`|Docker sẽ tạo ra một switch ảo. Khi container được tạo ra, interface của container sẽ gắn vào switch ảo này và kết nối với interface của host.|
|`host`| Containers sẽ dùng network trực tiếp của máy host. Network configuration bên trong container đồng nhất với host.|
|`overlay`|Overlay driver tạo ra một overlay network hỗ trợ cho các mạng multi-host. Được sử dụng kết hợp với Linux bridges và VXLAN để che đi liên lạc giữa các container qua cơ sở mạng hạ tầng vật lý.|
|`Macvlan`|MACVLAN driver sử dụng chế độ MACVLAN bridge để thiết lập kết nối giữa container interface và host interface (hoặc sub-interface). Nó có thể được sử dụng để cung cấp địa chỉ IP cho các container và định tuyến trên mạng vật lý. Ngoài ra VLANs có thể được trunked đến MACVLAN driver|
# Tương tác với network

|Câu lệnh|Mô tả|
|--------|-----|
|docker network ls|list cac network mặc định khi cài docker có 3 mạng là bridge, host, none|
|docker network create --driver [bridge/overlay/macvlan] --subnet [dải ip] [tên network] | tự định nghĩa mạng mới|
|docker network rm [tên network]|xóa network|

# Thực hành
## None network
Nếu muốn disable network của container có thể sử dụng `--network none` khi run container. Lúc này chỉ loopback đc tạo ra.
```
docker run -d --name none_nginx --network none nginx

...
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
       inet 127.0.0.1  netmask 255.0.0.0
       loop  txqueuelen 1  (Local Loopback)
       RX packets 0  bytes 0 (0.0 B)
       RX errors 0  dropped 0  overruns 0  frame 0
       TX packets 0  bytes 0 (0.0 B)
       TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
...

... 
"Networks": {
                "none": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "76d3252284e61180be206b533403621441bc13203a309ea71f299e004eef3410",
                    "EndpointID": "e066de60fa9a78075deae5665f5b82d2ab78dedb68dad24abf4b57fbbe2ac139",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
...
```
## Bridge network
 Bridge network sử dụng software bridge cho phép container kết nối với cùng bridge network để giao tiếp. Docker bridge driver tự động thiết lập rule trên host để các container khác bridge network không thể giao tiếp với nhau. Bridge network chỉ cho phép các container giao tiếp trên cùng một dockerd. Để giao tiếp giữa các container khác dockerd phải quản lý routing ở mức OS hoạc sử dụng overlay network. 

 Mặc định những container với default bridge thì không forward ra outside world. Để enable phải thay đổi cấu hình `sysctl net.ipv4.conf.all.forwarding=1` và `sudo iptables -P FORWARD ACCEPT` những cài đặt này sẽ không còn khi reboot vì vậy phải thêm vào 
start-up script.


Khi cài đặt docker một default bridge được tạo ra với docker0 172.17.0.1 và container khi chạy ko cấu hình network sẽ tự động sử dụng bridge này. IP của container sẽ được gán 172.17.0.x  
```
docker run -d --name default_nginx --network bridge nginx
 
... 
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "b16fb5e6dba627b0a19b623b053c19fbf02878cd78297704aea0dced4eccdfb8",
                    "EndpointID": "9f0c43ec04dcb49d31c009082f011f4a22d88fb6a14a0e4b9d0270b91c6144c9",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
...
```

Ta cũng có thể định nghĩa bridge network riêng `user-defined bridge network`
```
docker network create --driver brigde --subnet 192.168.100.0/24 my_bridge
docker run -d --name default_nginx --network my_bridge nginx
 
... 
"Networks": {
                "my_bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "c5a9f08bf803"
                    ],
                    "NetworkID": "d449f0f92dae90ffd433fc8b08a535dfcbb77e66da82f860f5afb28095de82b9",
                    "EndpointID": "bdabba0f647b5e0ef3f6f2064a1ce74cd3e7f8cd29153e23f11f6a31ceeb6556",
                    "Gateway": "192.168.100.1",
                    "IPAddress": "192.168.100.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:7f:00:00:02",
                    "DriverOpts": null
                }
            }

...
```

Sự khác biệt giữa user-defined bridge với default bridge
- Container với cùng user-defined bridge network sẽ tự động phơi tất cả cách port ra cho nhau và không port nào ra outside world. Điều này cho phép những ứng dụng giao tiếp với nhau dễ dàng mà không phải vô tình truy cập ra outside world. Với 
- Container với default bridge chỉ có thể truy cập với container khác thông qua IP. Với user-defined bridge network container có thể thông qua name hoặc alias.
- Trong lifetime của container bạn có thể onnect hoặc disconnect user-defined network `docker network connect my-bridge my_ubuntu` 
`docker network disconnect my-bridge my_ubuntu` .Còn với default network phải stop container và tạo lại với lựa chọn network khác. 
- Container sử dụng default bridge network có thể cấu hình được tuy nhiên tất cả container sử dụng default sẽ có cùng setting, thêm nữa khi cấu hình default bridge bên ngoài docker phải khởi động lại Docker. Sử dụng user-defined bridge thì việc cấu hình sẽ dễ dang hơn.
- Chính thức chỉ có duy nhất 1 cách để chia sẻ biến môi trường giữa 2 container bằng các link chúng sử dụng `--link` flag. Cách này sẽ bất khả thi với user-defined networks. Tuy nhiên có các khác để chia sẻ sử dụng docker volume, docker compose hoặc docker swarm.

## Host network
Nếu sử dụng host network thì container sẽ không cô lập với host, ví dụ nếu một ứng dụng sử dụng port 80 của container thì cũng sẽ lắng nghe trên port 80 của host.Containers sẽ dùng network trực tiếp của máy host. Network configuration bên trong container đồng nhất với host.Host networking driver chỉ làm việc trên Linux host và không hỗ trợ trên Mac, Window hay docker EE cho window server.
```
docker run - --name host_nginx --network host nginx

...
br-d449f0f92dae: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 127.0.0.1  netmask 255.255.0.0  broadcast 127.0.255.255
        inet6 fe80::42:d8ff:fe0d:b2b1  prefixlen 64  scopeid 0x20<link>
        ether 02:42:d8:0d:b2:b1  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8  bytes 648 (648.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:14ff:fee5:b1ce  prefixlen 64  scopeid 0x20<link>
        ether 02:42:14:e5:b1:ce  txqueuelen 0  (Ethernet)
        RX packets 36963  bytes 2577282 (2.5 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 51203  bytes 80212709 (80.2 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.40.150  netmask 255.255.255.0  broadcast 192.168.40.255
        inet6 fe80::5054:ff:feb6:b81c  prefixlen 64  scopeid 0x20<link>
        ether 52:54:00:b6:b8:1c  txqueuelen 1000  (Ethernet)
        RX packets 1410952  bytes 214610038 (214.6 MB)
        RX errors 0  dropped 5  overruns 0  frame 0
        TX packets 70934  bytes 13484762 (13.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 469  bytes 93789 (93.7 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 469  bytes 93789 (93.7 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

...
... 
 "Networks": {
                "host": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "ed30a30540e491a540555cf443bd6f0e8004d4269bc123df361de2a165363fa6",
                    "EndpointID": "06a4ab2ec643d76d7f195af5e095720e847364950218950a7205507a3ef7849b",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
...
```
## Macvlan network
Một vào app cần connect trực tiếp vơi physical network lúc này sử dụng Macvlan network để chỉ định địa chỉ MAC cho mỗi network interface của container
### Macvlan mode bridge
```
docker network create -d macvlan --subnet 192.168.40.0/24 --gateway 192.168.40.1 --ip-range=192.168.40.151/28 -o parent=ens3 macvlan0

```
### Macvlan mode vlan
Tạo 2 vlan 
```
docker network create -d macvlan --subnet 192.168.50.0/24 --gateway 192.168.50.1 -o parent=ens3.50 macvlan50
docker network create -d macvlan --subnet 192.168.60.0/24 --gateway 192.168.60.1 -o parent=ens3.60 macvlan60
```
Run 2 container sử dụng macvlan50, 2 container sử dụng macvlan60: 2 conatainer cùng macvlan sẽ ping đc tới nhau
