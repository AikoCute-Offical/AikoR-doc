# 1. Install Acme Script

Nhận máy chủ của bạn cập nhật và Ngoài ra cài đặt Curl và Socat:

* Ubuntu/Debian:
```bash
apt update && apt upgrade -y
```

```
apt install curl socat -y
```

* CentOS:
```bash
yum update -y
```

```
yum install curl socat -y
```




Tải xuống và cài đặt tập lệnh ACME để nhận chứng chỉ SSL miễn phí:

```
curl https://get.acme.sh | sh
```

## 2. Get a free SSL certificate

2.1 Đặt nhà cung cấp mặc định thành cho phép mã hóa:

```
~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
```

2.2 Đăng ký tài khoản của bạn cho chứng chỉ SSL miễn phí. Trong lệnh tiếp theo, thay thế xxxx@xxxx.com bằng địa chỉ email thực tế của bạn:

```
~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com
```

2.3 Có được chứng chỉ SSL. Trong lệnh tiếp theo, thay thế host.mydomain.com bằng tên máy chủ thực tế của bạn:

```
~/.acme.sh/acme.sh --issue -d host.mydomain.com --standalone
```

2.4 Sau một phút hoặc lâu hơn, kịch bản chấm dứt. Khi thành công, bạn sẽ nhận được phản hồi về vị trí của chứng chỉ và khóa:

```
Your cert is in: /root/.acme.sh/host.mydomain.com/host.mydomain.com.cer
Your cert key is in: /root/.acme.sh/host.mydomain.com/host.mydomain.com.key
The intermediate CA cert is in: /root/.acme.sh/host.mydomain.com/ca.cer
And the full chain certs is there: /root/.acme.sh/host.mydomain.com/fullchain.cer
```

2.5 Sao chép chứng chỉ và khóa vào thư mục Aikor:

```
cp /root/.acme.sh/host.mydomain.com/host.mydomain.com.cer /etc/AikoR/cert/host.mydomain.com.cer
cp /root/.acme.sh/host.mydomain.com/host.mydomain.com.key /etc/AikoR/cert/host.mydomain.com.key
```

2.6 Chỉnh sửa tệp cấu hình aikor config.yml và đặt các tham số sau

```
CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        RejectUnknownSni: false # Reject unknown SNI
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/AikoR/cert/host.mydomain.com.cer # Provided if the CertMode is file
        KeyFile: /etc/AikoR/cert/host.mydomain.com.key
        Provider: acme-dns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
```

# 3. Install AikoR 

* [Download and install](Download-and-install/install/README.md)
  * [Install One Link](Download-and-install/install/one-click.md)
  * [Using Docker HUB](Download-and-install/install/docker-hub.md)
  * [Using Docker Package](Download-and-install/install/docker-package.md)
  * [Manual installation](Download-and-install/install/manual.md)

