# Docker Engine
- Úng dụng client-server
- Một máy chủ là một loại chương trình dài chạy được gọi là quá trình daemon (lệnh dockerd).
- API REST chỉ định các giao diện mà các chương trình có thể sử dụng để nói chuyện với daemon và hướng dẫn nó làm gì.
- Giao diện dòng lệnh CLI ( docker command line ) CLI sử dụng API của Docker REST để điều khiển hoặc tương tác với 
daemon Docker thông qua kịch bản hoặc các lệnh CLI trực tiếp. Nhiều ứng dụng Docker khác sử dụng API cơ bản và CLI.
## 1.Cài đặt  
- Đăng nhập với quyên root và thực hiện lệnh dưới để cài đặt. Đảm bảo máy có kết nối internet.
```
su - 
curl -sSL https://get.docker.com/ | sudo sh
```
- Phân quyền cho user1 
```
sudo usermod -aG docker `user1`
```
## 2.Sử dụng
### 2.1.Thực hành với image
|Câu lệnh|Mô tả|
|--------|-----|
|docker search [name]| tìm kiếm image|
|docker pull [name]| pull image về máy local|
|docker images| #list các image có trong local|
|docker image inspect [name]| thông tin chi tiết image|
|docker history [name]|  lịch sử image |
|docker rmi [name]| xóa image|
|docker rmi -f $(docker images -qa)|xóa tất cả image|
|docker tag [old] [new]|đổi tên tag của iamge|
### 2.2.Thực hành với container
|Câu lệnh|Mô tả|
|--------|-----|
|docker ps|list các container đang chạy|
|docker ps -a|list tất cả các container|
|docker rm [name / ID]| xóa container|
|docker rm -f $(docker ps -qa)|xóa tất cả các container|
|docker run [name image]| chạy container từ image|
|docker run -d [name image]| container chạy nền|
|docker run -it [name image]|chạy và login vào container|
|docker run -p [host post]:[container host]|gán port cho container với host|
