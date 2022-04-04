# CONTAINER HÓA MÁY CHỦ WEB

## Nội dung

- ### Lý thuyết

  - [Cài đặt Docker :whale: trên máy chủ Ubuntu :penguin:](#install-docker-ubuntu)[^1]

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker networking :globe_with_meridians:](#docker-netwoking)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker storage (volumes)](#docker-volume)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Dockerfile](#Dockerfile)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker-compose](#docker-compose)

- ### Thực hành 

  - [Cài đặt WORDPRESS từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#wordpress)

  - [Cài đặt MYSQL từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#mysql)

  - [Cài đặt PHPMYADMIN từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#phpmyadmin)

  - [Sau khi đã hoàn tất 3 yêu cầu trên hãy tạo một Docker-compose để tự hóa việc cài đặt cho 3 dịch vụ trên](#usage-docker-compose)

  - [Cài đặt Portainer để quản lý Docker](#portainer)

*** 

## Cài đặt Docker :whale: trên máy chủ Ubuntu :penguin: <a id="install-docker-ubuntu"></a>

    sudo apt-get update
>
    sudo apt-get install ca-certificates curl gnupg lsb-release
>
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
>
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
>
    sudo apt-get update
>
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose

*** 

## Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker networking :globe_with_meridians: <a id="docker-netwoking"></a>

### 1. Khái niệm

Docker network sẽ đảm nhiệm vụ kết nối mạng giữa các container với nhau, kết nối giữa container với bên ngoài, cũng như kết nối giữa các cụm (swarm) docker containers.

### 2. Một số lệnh cơ bản

- Liệt kê các network đang có:
    
        docker network ls

- Tạo một network mới:

        docker network create -d `network-driver` `name-network`

    > Trong đó:
    >
    > network-driver: bridge, host, overlay, ipvlan, macvlan, none
    > 
    > name-network: tự đặt

[^1]: https://docs.docker.com/engine/install/ubuntu/




