# Docker network  

|Driver|Mô tả|
|------|-----|
|`none`|Các container thiết lập network này sẽ không được cấu hình mạng.|
|`bridge`|Docker sẽ tạo ra một switch ảo. Khi container được tạo ra, interface của container sẽ gắn vào switch ảo này và kết nối với interface của host.|
|`host`| Containers sẽ dùng mạng trực tiếp của máy host. Network configuration bên trong container đồng nhất với host.|
|`overlay`|Overlay driver tạo ra một overlay network hỗ trợ cho các mạng multi-host. Được sử dụng kết hợp với Linux bridges và VXLAN để che đi liên lạc giữa các container qua cơ sở mạng hạ tầng vật lý.|
|`Macvlan`|MACVLAN driver sử dụng chế độ MACVLAN bridge để thiết lập kết nối giữa container interface và host interface (hoặc sub-interface). Nó có thể được sử dụng để cung cấp địa chỉ IP cho các container và định tuyến trên mạng vật lý. Ngoài ra VLANs có thể được trunked đến MACVLAN driver|
# Tương tác với network

|Câu lệnh|Mô tả|
|--------|-----|
|docker network ls|list cac network mặc định khi cài docker có 3 mạng là bridge, host, none|
|docker network create --driver [bridge/overlay/macvlan] --subnet [dải ip] [tên network] | tự định nghĩa mạng mới|
|docker network rm [tên network]|xóa network|

# Thực hành
## Tạo container với none network
```
 docker run -d --name none_nginx --network none nginx
 
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
## Tạo container với default bridge
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
## Tạo container với user-defined bridge
```

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
                    "Gateway": "127.0.0.1",
                    "IPAddress": "127.0.0.2",
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

