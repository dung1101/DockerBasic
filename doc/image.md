# 1.Docker image
`image` là một read-only template để tạo ra Docker `container`. Thông thường một image được xây dựng trên nền một image khác. Để tạo một image sử dụng `Dockerfile` với những cú pháp đơn giản định nghĩa các bước cần thiết để tạo và chạy image. Mỗi một chỉ dẫn trong dockerfile sẽ tạo một lớp (layer) trên image, các lớp này xếp chồng lên nhau. Khi thay đổi dockerfile và build lại thì chỉ nhũng lớp nào thay đổi sẽ build lại. Điều này làm cho image nhỏ và nhanh hơn so với công nghệ ảo hóa khác. Ngoài ra có cách khác để tạo image là chạy 1 container sau đó chạy các câu lệnh cần thiết và sử dụng `docker commit` để tạo image.

Các thao tác với image

|Câu lệnh|Mô tả|
|--------|-----|
|docker search [name]| tìm kiếm image|
|docker pull [name]| pull image về máy local|
|docker images| list các image có trong local|
|docker image inspect [name]| thông tin chi tiết image|
|docker history [name]|  lịch sử image |
|docker rmi [name]| xóa image|
|docker rmi -f $(docker images -qa)|xóa tất cả image|
|docker tag [old] [new]|đổi tên tag của image|

# 2.Docker container
Container là một gói phần mềm thực thi lightweight, độc lập và có thể thực thi được. Container bao gồm mọi thứ cần thiết để chạy được nó: code, runtime, system tools, system libraries, settings. Các container sẽ luôn chạy ổn định bất kể môi trường.
Nhiều container có thể chạy trên cùng một máy và chia sẻ kernel của hệ điều hành với các container khác, mỗi máy đều chạy như các quá trình bị cô lập trong không gian người dùng. Các container chiếm ít không gian hơn các máy ảo (container image thường có vài trăm thậm chí là vài MB), và start gần như ngay lập tức.

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
|docker logs [name]|xem log cua container|
|docker service logs [service]|xem log của service|

# 3.Docker manage data
By default all files created inside a container are stored on a writable container layer. This means that:
- The data doesn’t persist when that container is no longer running, and it can be difficult to get the data out of the container if another process needs it.
- A container’s writable layer is tightly coupled to the host machine where the container is running. You can’t easily move the data somewhere else.
- Writing into a container’s writable layer requires a storage driver to manage the filesystem. The storage driver provides a union filesystem, using the Linux kernel. This extra abstraction reduces performance as compared to using data volumes, which write directly to the host filesystem.

Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts. If you’re running Docker on Linux you can also use a tmpfs mount.
- Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
- Bind mounts may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.
- tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

![img](./imgs/types-of-mounts-tmpfs.png)
## 3.1.Volumes
Volumes: Created and managed by Docker. You can create a volume explicitly using the docker volume create command, or Docker can create a volume during container or service creation.

When you create a volume, it is stored within a directory on the Docker host. When you mount the volume into a container, this directory is what is mounted into the container. This is similar to the way that bind mounts work, except that volumes are managed by Docker and are isolated from the core functionality of the host machine.

A given volume can be mounted into multiple containers simultaneously. When no running container is using a volume, the volume is still available to Docker and is not removed automatically. You can remove unused volumes using docker volume prune. When remove/change data in a volume, datas in containers bind with that volume don't change (difference with bind-mount). 

When you mount a volume, it may be named or anonymous. Anonymous volumes are not given an explicit name when they are first mounted into a container, so Docker gives them a random name that is guaranteed to be unique within a given Docker host. Besides the name, named and anonymous volumes behave in the same ways.

Volumes also support the use of volume drivers, which allow you to store your data on remote hosts or cloud providers, among other possibilities.

### Good use cases for volumes
Volumes are the preferred way to persist data in Docker containers and services. Some use cases for volumes include:
- Sharing data among multiple running containers. If you don’t explicitly create it, a volume is created the first time it is mounted into a container. When that container stops or is removed, the volume still exists. Multiple containers can mount the same volume simultaneously, either read-write or read-only. Volumes are only removed when you explicitly remove them.
- When the Docker host is not guaranteed to have a given directory or file structure. Volumes help you decouple the configuration of the Docker host from the container runtime.
- When you want to store your container’s data on a remote host or a cloud provider, rather than locally.
- When you need to back up, restore, or migrate data from one Docker host to another, volumes are a better choice. You can stop containers using the volume, then back up the volume’s directory (such as `/var/lib/docker/volumes/<volume-name>`).

|Câu lệnh|Mô tả|
|--------|-----|
|docker volume create [name vol]|tạo volume|
|docker volume ls|list volume|
|docker volume inspect [name volume]|chi tiết volume|
|docker volume rm [name vol]|xóa volume|
|--mount 'type=volume,src=[VOLUME-NAME],dst=[CONTAINER-PATH]'|option mount volume khi run container|
|-v [VOLUME-NAME]:[CONTAINER-PATH]| similar above|

## 3.2.Bind mounts
Bind mounts: Available since the early days of Docker. Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full path on the host machine. The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. Bind mounts are very performant, but they rely on the host machine’s filesystem having a specific directory structure available. If you are developing new Docker applications, consider using named volumes instead. You can’t use Docker CLI commands to directly manage bind mounts.
|Câu lệnh|Mô tả|
|--------|-----|
|--mount 'type=bind,src=[HOST-PATH],dst=[CONTAINER-PATH]'|option mount volume khi run container|
|-v [HOST-PATH]:[CONTAINER-PATH]|  similar above|
### Good use cases for bind mounts
In general, you should use volumes where possible. Bind mounts are appropriate for the following types of use case:

- Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting /etc/resolv.conf from the host machine into each container.
- Sharing source code or build artifacts between a development environment on the Docker host and a container. For instance, you may mount a Maven target/ directory into a container, and each time you build the Maven project on the Docker host, the container gets access to the rebuilt artifacts.
- If you use Docker for development this way, your production Dockerfile would copy the production-ready artifacts directly into the image, rather than relying on a bind mount.
- When the file or directory structure of the Docker host is guaranteed to be consistent with the bind mounts the containers require.

## 3.3.tmpfs mounts
A tmpfs mount is not persisted on disk, either on the Docker host or within a container. It can be used by a container during the lifetime of the container, to store non-persistent state or sensitive information. For instance, internally, swarm services use tmpfs mounts to mount secrets into a service’s containers

### Good use cases for tmpfs mounts
`tmpfs` mounts are best used for cases when you do not want the data to persist either on the host machine or within the container. This may be for security reasons or to protect the performance of the container when your application needs to write a large volume of non-persistent state data.

## Tips for using bind mounts or volumes
If you use either bind mounts or volumes, keep the following in mind:

- If you mount an empty volume into a directory in the container in which files or directories exist, these files or directories are propagated (copied) into the volume. Similarly, if you start a container and specify a volume which does not already exist, an empty volume is created for you. This is a good way to pre-populate data that another container needs.

- If you mount a bind mount or non-empty volume into a directory in the container in which some files or directories exist, these files or directories are obscured by the mount, just as if you saved files into /mnt on a Linux host and then mounted a USB drive into /mnt. The contents of /mnt would be obscured by the contents of the USB drive until the USB drive were unmounted. The obscured files are not removed or altered, but are not accessible while the bind mount or volume is mounted.

