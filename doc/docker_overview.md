# Docker 
## 1.Overview
Docker là một nền tảng mã nguồn mở cung cấp cách để building, deploying và running ứng dụng một cách dễ dàng trên nền tảng ảo hóa containerlization. Docker cho phép bạn tách ứng dụng khỏi nền tảng để có thể triển khai nhanh chóng.  Docker có 2 phiên bản CE (Community Edition) và EE (Enterprise Edition).
## 2.Docker Engine
Docker engine là một client-server application bao gồm các phần chính:
- server là một long-running program gọi là daemon process (the dockerd command).
- giao diện dòng lệnh (CLI) client (the docker command).
- API cho phép client tương tác với deamon.
[img]
## 3.Kiến trúc docker
Sử dụng kiến trúc client-server, client và deamon có thể ở trên cùng một hệ thống hoặc có thể kết nối client đến remote deamon
[img]
### 3.1.The Docker daemon (dockerd) 
lắng nghe request từ docker API và quản lý docker objects như image, container, network và volume. Daemon cũng có thể giao tiếp với các deamon khác để quản lý các service.
### 3.2.The Docker client (docker) 
clent sữ gửi những command đến dockerd thông qua API. Một client có thể giao tiếp với nhiều deamon.
### 3.3.Docker registries
Docker registry lưu trữ Docker images. Docker Hub và Docker Cloud là nhưng public registry mà mọi người có thể sử dụng.Mặc định Docker sẽ tìm kiếm image trên Docker Hub.

