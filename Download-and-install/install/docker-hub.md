# Cài đặt bằng docker

địa chỉ gương：https://github.com/AikoR-project/AikoR/pkgs/container/AikoR
## docker image tag

`master`: Phù hợp với cam kết mới nhất của dự án.

`latest`: Phiên bản phát hành mới nhất.

`v*`: release số phiên bản。

## Cài đặt Docker

### Centos

```bash
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
```

### Debian / Ubuntu

```bash
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
```

## Cài đặt Docker-compose

```bash
curl -fsSL https://get.docker.com | bash -s docker
curl -L "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## Docker-compose Cài đặt AikoR \(giới thiệu\)

1. `git clone https://github.com/AikoCute-Offical/AikoR-Docker-Hub.git`
2. `cd AikoR-Docker-Hub`
3. Chỉnh sửa tệp cấu hình：`aiko.yml`，xem chi tiết：[Mô tả tệp cấu hình](../../Configuration-file-description/config.md)
4. bắt đầu docker：`docker-compose up -d`

## Docker chạy cài đặt AikoR

Vui lòng lưu ý các chỉ định `aiko.yml` Mục lục。

```bash
docker pull aikocute/aikor:latest && docker run --restart=always --name aikor -d -v ${PATCH_TO_CONFIG}/aiko.yml:/etc/AikoR/aiko.yml --network=host aikocute/aikor:latest
```

## Cập nhật AikoR

docker-comp có thể được cập nhật chỉ với hai lệnh đơn giản và chung chung、xóa vùng chứa và khởi động lại。Sau khi cập nhật phần mềm `aiko.yml` sẽ không bị ghi đè bởi các bản cập nhật.

Lưu ý rằng thực thi trong thư mục chứa docker-compost.yml:

```bash
docker-compose pull
docker-compose up -d
```

