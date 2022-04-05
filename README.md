# CONTAINER HÓA MÁY CHỦ WEB

## Nội dung

- ### Lý thuyết

  - [Cài đặt Docker :whale: trên máy chủ Ubuntu :penguin:](#install-docker-ubuntu)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker networking :globe_with_meridians:](#docker-netwoking)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker storage (volumes) :file_folder: ](#docker-volume)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Dockerfile](#Dockerfile)

  - [Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker-compose](#docker-compose)

- ### Thực hành 

  - [Cài đặt WORDPRESS từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#wordpress)

  - [Cài đặt MYSQL từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#mysql)

  - [Cài đặt PHPMYADMIN từ images và tạo container từ images đã cài đặt (Từ Docker hub và Dockerfile)](#phpmyadmin)

  - [Sau khi đã hoàn tất 3 yêu cầu trên hãy tạo một Docker-compose để tự hóa việc cài đặt cho 3 dịch vụ trên](#usage-docker-compose)

  - [Cài đặt Portainer để quản lý Docker](#portainer)

*** 

## Cài đặt Docker :whale: trên máy chủ Ubuntu :penguin:[^1] <a id="install-docker-ubuntu"></a>

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

### 1. Khái niệm [^2][^5]

Docker network sẽ đảm nhiệm vụ kết nối mạng giữa các container với nhau, kết nối giữa container với bên ngoài, cũng như kết nối giữa các cụm (swarm) docker containers.

### 2. Một số lệnh cơ bản [^3][^4]

- Liệt kê network:
    
      docker network ls

- Kiểm tra thông tin của network:

      docker network inspect [network-name]

- Port mapping:

      docker run -p [Host-Port]:[Client-Port] ...
    
    > Ví dụ: `docker run --name nginx -p 80:80 nginx`
    >
    > Cổng 80 của máy host sẽ ánh xạ vào cổng 80 của container nginx. Vậy khi chúng ta truy cập vào địa chỉ ip máy host cổng 80 tức là đang truy cập vào container nginx cổng 80

- Tạo network mới:

      docker network create -d [network-driver] [network-name]

    > Trong đó:
    >
    > `network-driver`: bridge, host, overlay, ipvlan, macvlan, none
    > 
    > `network-name`: tự đặt

- Xóa một network:

      docker network rm [network-name]

- Chỉ định container kết nối vào network khi khởi tạo:
      
      docker run --network [network-name] ...

    > Ví dụ: `docker run --name nginx --network mynetwork nginx`
    > 
    > container nginx thay vì mặc định sẽ kết nối với network `bridge` thì nó sẽ được chỉ định kết nối với network `mynetwork` 

- Kết nối container vào 1 network:

      docker network connect `[network-name] [container-name]

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

      docker network disconnect [network-name] [container-name]

***

## Tìm hiểu khái niệm và thực hiện 1 ví dụ về Docker storage (volumes) :file_folder: <a id="docker-volume"></a>

### 1. Khái niệm [^6][^7]

### 2. Một số lệnh cơ bản [^8]

- Chia sẻ dữ liệu giữa máy host và container:

      docker run -v [path-host]:[path-container] ....

  > Ví dụ: `docker run -itd --name B1 -v /home/fit:/home/data busybox`
  >
  > Chúng ta đã chia sẻ dữ liệu thành công giữa folder `/home/fit` của máy host với folder `/home/data` của container B1. Tức là folder `/home/fit` và folder `/home/data` có dữ liệu y như nhau. Hơn thế nữa, nếu container B1 ghi dữ liệu vào folder `/home/data` thì folder `/home/fit` sẽ được tự động cập nhật theo. Khi chúng ta xóa container B1 thì dữ liệu trong folder `/home/fit` sẽ không bị mất

***

[^1]: https://docs.docker.com/engine/install/ubuntu/
[^2]: https://viblo.asia/p/docker-networking-nhung-khai-niem-va-cach-su-dung-co-ban-gGJ59P2JlX2
[^3]: https://www.youtube.com/watch?v=k1SwXOxvMdE&list=PLwJr0JSP7i8At14UIC-JR4r73G4hQ1CXO&index=5
[^4]: https://docs.docker.com/engine/reference/commandline/network/
[^5]: https://docs.docker.com/network/
[^6]: https://blog.cloud365.vn/container/tim-hieu-docker-phan-7/
[^7]: https://viblo.asia/p/docker-co-ban-p2-storage-gAm5yVo8Kdb
[^8]: https://www.youtube.com/watch?v=DSP2-ip38Zw&list=PLwJr0JSP7i8At14UIC-JR4r73G4hQ1CXO&index=4







