# Docker network  
## `none`: Các container thiết lập network này sẽ không được cấu hình mạng.
```
$ docker run --network none ubuntu:latest 
```
## `bridge`: Docker sẽ tạo ra một switch ảo. Khi container được tạo ra, interface của container sẽ gắn vào switch ảo này và kết nối với interface của host.
## `host`: Containers sẽ dùng mạng trực tiếp của máy host. Network configuration bên trong container đồng nhất với host.
## `overlay`: 
## `Macvlan`:
