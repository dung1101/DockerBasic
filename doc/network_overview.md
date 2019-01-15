## Container Networking Model (CNM)
![img](./imgs/docker-network-models.png)

Đây là cấu trúc mức độ cao trong CNM. Theo đó, ta có:
- Sandbox: Chứa các cấu hình của ngăn xếp mạng container. Bao gồm quản lý network interface, route table và các thiết lập DNS. Một Sandbox có thể được coi là một namespace network và có thể chứa nhiều endpoit từ nhiều mạng.
- Endpoint: Là điểm kết nối một Sandbox tới một mạng.
- Network: CNM không chỉ định một mạng tuân theo mô hình OSI. Việc triển khai mạng có thể là VLAN, Bridge, ... Các endpoint không kết nối với mạng thì không có kết nối trên mạng.
- CNM cung cấp 2 interface có thể được sử dụng cho việc liên lạc, điều khiển, ... container trong mạng:
    - Network Drivers: Cung cấp, thực hiện thực tế việc tạo ra một mạng hoạt động. Được sử dụng với các drivers khác và thay đổi một cách dễ dàng đối với các trường hợp cụ thể. Nhiều network driver có thể được sử dụng trong Docker nhưng mỗi một network chỉ là một khởi tạo từ một network driver duy nhất. Theo đó mà ta có 2 loại chính của CNM network drivers như sau:
        - Native Network Drivers: Là một phần của Docker Engine và được cung cấp bới Docker. Có nhiều drivers để dễ dàng lựa chọn cho khả năng của mạng như overlay networks hay local bridges.
        - Remote Network Drivers: Là các network drivers được tạo ra bởi cộng đồng và các nhà cung cấp. Được sử dụng để tích hợp vào các phần mềm hoặc phần cứng đang hoạt động.
    - IPAM Drivers: Drivers quản lý các địa chỉ IP cung cấp mặc định cho các mạng con hoặc địa chỉ IP cho các mạng và endpoint nếu chúng không được chỉ định. Địa chỉ IP cũng có thể gán thủ công qua mạng, container, ...

## Network driver  
|Driver|Mô tả|
|------|-----|
|`bridge`|Driver mặc định. Nếu không chỉ định cụ thể thì driver này sẽ được sử dụng. Bridge network thường được sử dụng khi các container đọc lập muốn giao tiếp với nhau|
|`host`| Xóa bỏ sự cô lập giữa container và host và sử dụng trục tiếp network của host. Host network chỉ sử dụng cho swarm trên Docker 17.06 và cao hơn|
|`overlay`| overlay network kết nối nhiều docker deamons lại và cho phép các swarm service giao tiếp với nhau.Có thể sử dụng overlay network để giao tiếp giữa swarm service và container hay giữa 2 container trên các docker deamon khác nhau |
|`macvlan`| Macvlan network cho phép gán một địa chỉ MAC cho một container làm nó trở thành một thiết bị vật lý trên mạng.Docker daemon định tuyến đến container thông qua địa chỉ MAC của chúng.Đây là lựa chọn tốt nhất khi một ứng dụng muốn kết nối trực tiếp dến mạng vật lý hơn là định tuyến thông qua docker host|
|`none`|Vô hiệu hóa network. none network không sử dụng được cho swarm service |
|Network plugins|Có thể sử dụng plugin của bên thứ 3|

