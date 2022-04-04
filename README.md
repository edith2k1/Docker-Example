# CONTAINER HÓA MÁY CHỦ WEB

## Nội dung

- ### Lý thuyết

  - [Cài đặt Docker :whale: trên máy chủ Ubuntu :penguin:](#install-docker-ubuntu)[^1]

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker networking :globe_with_meridians:](#docker-netwoking)[^2]

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

- Liệt kê network:
    
      docker network ls

- Kiểm tra thông tin của network:

      docker network inspect `network-name`

- Port mapping:

      docker run -p `Host-Port`:`Client-Port` ...
    
    > Ví dụ: `docker run --name nginx -p 80:80 nginx`
    >
    > Cổng 80 của máy host sẽ ánh xạ vào cổng 80 của container nginx. Vậy khi chúng ta truy cập vào địa chỉ ip máy host cổng 80 tức là đang truy cập vào container nginx cổng 80

- Tạo network mới:

      docker network create -d `network-driver` `network-name`

    > Trong đó:
    >
    > `network-driver`: bridge, host, overlay, ipvlan, macvlan, none
    > 
    > `network-name`: tự đặt

- Xóa một network:

      docker network rm `network-name`

- Chỉ định container kết nối vào network khi khởi tạo:
      
      docker run --network `network-name` ...

    > Ví dụ: `docker run --name nginx --network mynetwork nginx`
    > 
    > container nginx thay vì mặc định sẽ kết nối với network `bridge` thì nó sẽ được chỉ định kết nối với network `mynetwork` 

- Kết nối container vào 1 network:

      docker network connect `network-name` `container-name`

    > Ví dụ: 
    >
    > `docker run --name nginx nginx`
    >
    > => container nginx mặc định kết nối vào network `bridge`
    > 
    > `docker network connect mynetwork nginx`
    >
    > => container nginx được kết nối vào network `mynetwork`
    >
    > => Lúc này, container nginx kết nối vào 2 network: `bridge` và `mynetwork`

- Ngắt kết nối container với network:

      docker network disconnect `network-name` `container-name`

***



[^1]: https://docs.docker.com/engine/install/ubuntu/
[^2]: https://docs.docker.com/engine/reference/commandline/network/




