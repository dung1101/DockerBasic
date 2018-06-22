# Network namespace
Network namspace là khái niệm cho phép bạn cô lập môi trường mạng network trong một host.
Namespace phân chia việc sử dụng các khác niệm liên quan tới network như devices, địa chỉ
addresses, ports, định tuyến và các quy tắc tường lửa vào trong một hộp (box) riêng biệt,
chủ yếu là ảo hóa mạng trong một máy chạy một kernel duy nhất.
Mỗi network namespaces có bản định tuyến riêng, các thiết lập iptables riêng cung cấp cơ chế NAT
và lọc đối với các máy ảo thuộc namespace đó. Linux network namespaces cũng cung cấp thêm khả năng 
để chạy các tiến trình riêng biệt trong nội bộ mỗi namespace.
# Cgroups 
C-groups cho phép bạn có thể cấp phát tài nguyên – như thời gian sử dụng CPU, bộ nhớ hệ thống,
băng thông mạng. Bạn có thể theo dõi cgroups, từ chối cgroups sử dụng tài nguyên nhất định và 
ngay cả việc cấu hình lại cgroups trên một hệ thống đang chạy.
Sử dụng cgroups, người quản trị hệ thống có thể điều khiển các việc như cấp phát, ưu tiên, từ chối, 
quản lí và theo theo dõi tài nguyên hệ thống. Tài nguyên phần cứng sẽ được chia sẽ hiệu quả giữa 
các người dùng và tăng khả năng ổn định của hệ thống.
