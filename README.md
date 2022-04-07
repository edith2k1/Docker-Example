# CONTAINER HÓA MÁY CHỦ WEB :triumph:

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

Docker network sẽ đảm nhiệm nhiệm vụ kết nối mạng giữa các container với nhau, kết nối giữa container với bên ngoài, cũng như kết nối giữa các cụm (swarm) docker containers.

### 2. Một số lệnh cơ bản [^3][^4]

- Liệt kê network:
    
      docker network ls

- Kiểm tra thông tin của network:

      docker network inspect [network-name]

- Port mapping:

      docker run -p [Host-Port]:[Client-Port] ...
    
    > Ví dụ:
    > 
    > `docker run --name nginx -p 80:80 nginx`
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

    > Ví dụ:
    > 
    > `docker run --name nginx --network mynetwork nginx`
    > 
    > container nginx thay vì mặc định sẽ kết nối với network `bridge` thì nó sẽ được chỉ định kết nối với network `mynetwork` 

- Kết nối container vào 1 network:

      docker network connect [network-name] [container-name]

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

Docker storage sẽ đảm nhiệm nhiệm vụ quản lý data của các container, chia sẻ dữ liệu giữa máy host và container, giữa các container với nhau.

### 2. Một số lệnh cơ bản [^8]

- Chia sẻ dữ liệu giữa máy host và container:

      docker run -v [path-host]:[path-container] ....

  > Ví dụ:
  >
  > `docker run -itd --name B1 -v /home/fit:/home/data busybox`
  >
  > Chúng ta đã chia sẻ dữ liệu thành công giữa folder `/home/fit` của máy host với folder `/home/data` của container B1. Tức là folder `/home/fit` và folder `/home/data` có dữ liệu y như nhau. Hơn thế nữa, dữ liệu trong folder `/home/data` và folder `/home/fit` sẽ được tự động đồng bộ với nhau. Cuối cùng, khi chúng ta xóa container B1 thì dữ liệu trong folder `/home/fit` sẽ không bị mất

- Chia sẻ dữ liệu giữa các container:

      docker run --volumes-from [container-name] ...

    > Ví dụ: 
    >
    > `docker run -itd --name B2 --volumes-from B1 busybox`
    >
    > Chúng ta đã chia sẻ dữ liệu thành công giữa 2 container B1 và B2. Tức là trong container B2 cũng sẽ có folder `/home/data` và dữ liệu trong folder này so với folder `/home/data` của container B1 là như nhau. Hơn thế nữa, dữ liệu trong cả 2 folder này được tự động đồng bộ với nhau

- Liệt kê volume:

      docker volume ls

- Tạo volume mới:

      docker volume create [volume-name]

- Kiểm tra thông tin của volume:

      docker network inspect [volume-name]

- Xóa volume:

      docker volume rm [volume-name]

- Gắn volume vào container:

  - -v flag:

        docker run -v [volume-name]:[container-path] ...

  - --mount flag:

        docker run --mount src=[volume-name],dst=[container-path] ...

      > Ví dụ:
      >
      > `docker run -it --name B1 -v D1:/home/B1 busybox`
      >
      > Hoặc
      >
      > `docker run -it --name B1 --mount src=D1,dst=/home/B1 busybox`
      > 
      > Chúng ta đã gắn thành công volume D1 vào container B1. Volume D1 được ánh xạ vào folder `/home/B1`. Có nghĩa là, dữ liệu trong folder `/home/B1` sẽ được đồng bộ với volume D1. Khi chúng ta xóa container B1 thì dữ liệu trong volume D1 không bị mất


- Tạo volume ánh xạ đến thư mục máy host:

      docker volume create --opt device=[host-path] --opt type=none --opt o=bind [volume-name]

  > Ví dụ: 
  >
  > `docker volume create --opt device=/home/fit --opt type=none --opt o=bind DISK`
  >
  > Tạo thành công volume DISK ánh xạ đến folder `/home/fit` trên máy host.
  >
  > ` docker run -it --name B6 -v DISK:/home/B6 busybox`
  >
  > Gắn volume DISK vào container B6
  > 
  > => volume DISK - folder `/home/fit` máy host - folder `/home/B6` container B6 => **đồng bộ**

***

## Tìm hiểu khái niệm và thực hiện 1 ví dụ về Dockerfile <a id="Dockerfile"></a>

### 1. Khái niệm [^9]

Dockerfile là một file dạng text không có extension, và tên bắt buộc phải là Dockerfile

Dockerfile là một file kịch bản sử dụng để tạo mới một image

### 2. Một số ví dụ cơ bản

***

[^1]: https://docs.docker.com/engine/install/ubuntu/
[^2]: https://viblo.asia/p/docker-networking-nhung-khai-niem-va-cach-su-dung-co-ban-gGJ59P2JlX2
[^3]: https://www.youtube.com/watch?v=k1SwXOxvMdE&list=PLwJr0JSP7i8At14UIC-JR4r73G4hQ1CXO&index=5
[^4]: https://docs.docker.com/engine/reference/commandline/network/
[^5]: https://docs.docker.com/network/
[^6]: https://blog.cloud365.vn/container/tim-hieu-docker-phan-7/
[^7]: https://viblo.asia/p/docker-co-ban-p2-storage-gAm5yVo8Kdb
[^8]: https://www.youtube.com/watch?v=DSP2-ip38Zw&list=PLwJr0JSP7i8At14UIC-JR4r73G4hQ1CXO&index=4
[^9]: https://blog.cloud365.vn/container/tim-hieu-docker-phan-4/







