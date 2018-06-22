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