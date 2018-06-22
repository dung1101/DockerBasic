# Docker file
Dockerfile đơn giản là một file (dạng text nhưng không có Extension) chứa một tập hợp các dòng lệnh dùng để khởi tạo một docker image. 
Nó quy định image sẽ được khởi tạo như thế nào , gồm các ứng dụng gì trong đó.Trong Dockerfile có các câu lệnh chính sau <br>
FROM: xây dựng image trên các image có sẵn
```
FROM ubuntu14.04:lastest
```
RUN: chạy câu lệnh để cài đặt các service cần thiết
```
RUN apt-get update
RUN apt-get install nginx
```
CMD: chạy các service sau khi build
```
CMD curl
```
LABEL: để mô tả cho image
```
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
```
MAINTAINER: tên tác giả
```
MAINTAINER dungnguyen
```
EXPOSE: thông báo cho Docker rằng image sẽ lắng nghe trên các cổng được chỉ định khi chạy. 
Lưu ý là cái này chỉ để khai báo, chứ ko có chức năng nat port từ máy host vào container. 
Muốn nat port, thì phải sử dụng cờ -p (nat một vài port) hoặc -P (nat tất cả các port được khai báo trong EXPOSE) 
trong quá trình khởi tạo contrainer.
```
EXPOSE 80
```
ADD:Chỉ thị ADD copy file, thư mục, remote files URL (src) và thêm chúng vào filesystem của image (dest)

