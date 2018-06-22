# Docker 
## 1.Vai trò
Docker là một nền tảng mã nguồn mở cung cấp cách để building, deploying và running ứng dụng một cách dễ dàng trên nền tảng ảo hóa containerlization. 
## 2.Thành phần
Docker bao gồm:
- Docker Engine: Chứa các tool, engine để có thể đóng gói chương trình và vận hành chúng một cách đơn giản nhất.
- Docker Hub: Giống như Github, là nơi bạn có thể lưu trữ các chương trình đã được đóng gói (image) của mình và quản lý các image.
## 3.Các khái niệm
### 3.1.Docker image và Docker container
Hiểu theo lập trình hướng đối tượng thì Image được xem như là một class, chứa các phương thức và thuộc tính. 
Còn container như một thực thể (instance) của class đó.
Còn hiểu theo kiểu của Docker thì image là một package chứa tất tần tật những thứ cần thiết cho ứng dụng như thư viện, 
biến môi trường, mã nguồn, file cấu hình…Chỉ cần run image một cái là sẽ build ra container tương ứng phục vụ cho ứng dụng của bạn, 
mọi hoạt động trong container này sẽ không ảnh hưởng đến container khác, và sau khi sử dụng nếu không cần thiết có thể xóa bỏ.
### 3.2.Dockerfile
Vậy docker image tạo ra từ đâu, nó được tạo ra từ một đống câu lệnh được tổng hợp lại trong một file gọi là Dockerfile, 
sau khi build Dockerfile sẽ tạo ra docker image.
### 3.3.Docker compose
Compose là công cụ giúp định nghĩa và khởi chạy multi-container Docker applications.
Trong Compose, chúng ta sử dụng Compose file để cấu hình application's services. 
Chỉ với một câu lệnh, lập trình viên có thể dễ dàng create và start toàn bộ các services phục vụ cho việc chạy ứng dụng.
### 3.4.Docker volume
Là cơ chế tạo và sử dụng dữ liệu của docker, đã liên quan đến tạo và sử dụng dữ liệu thì bạn hoàn toàn có thể backup hoặc restore volume. 
Sử dụng volume để:
Bảo toàn dữ liệu khi một container bị xóa
Chia sẻ dữ liệu giữa máy chủ (host) và container
Chia sẻ dữ liệu giữa các container với nhau
Volume là cách bạn chỉ định đường dẫn (một command line) trong Docker compose xem đâu là nơi sẽ lưu trữ và chia sẻ data với container. 
Quay lại với ví dụ căn chung cư, thì bể nước để cung cấp nước cho căn hộ có thể hiểu như volume, và cách bạn bố trí đường ống chính 
là cách cấu hình đường dẫn trong file docker-compose.
### 3.5.Docker Network
Docker network dùng để gắn địa chỉ ip cho các container thông qua một virtual bridge.
### 3.6.Docker Swam
Docker swarm là một công cụ giúp chúng ta tạo ra một clustering Docker. 
Nó giúp chúng ta gom nhiều Docker Engine lại với nhau và ta có thể "nhìn" nó như duy nhất một virtual Docker Engine.
Khi số lượng containers tăng lên việc quản lý, triển khai trở nên phức tạp hơn thì không thể không sử dụng Docker Swarm.
