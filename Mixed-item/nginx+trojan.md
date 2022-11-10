# Nginx+Trojan！

sử dụngNginxđối phó vớiTrojancủaTLS，Trojanthực hiện một pullback。Tôi muốn gọi anh ấy là một vị thần trong lúc này！

## NginxCài đặt

CentOS：

```text
 yum update
 yum install -y nginx
 yum install nginx-mod-stream
```

Ubuntu/Debian:

```text
 apt update
 apt install nginx
```

## Nginxcấu hình

Ôn lại/etc/nginx/nginx.conf tập tin cấu hình：

```text
stream {
    server {
        listen              443 ssl;                    # Đặt cổng nghe thành 443

        ssl_protocols       TLSv1.2 TLSv1.3;      # thiết lập bằng cách sử dụngSSLPhiên bản giao thức

        ssl_certificate /etc/nginx/ssl/xx.com.pem; # Địa chỉ chứng chỉ
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # địa chỉ chính
        ssl_session_cache   shared:SSL:10m;             # SSL TCPBộ đệm phiên đặt vùng bộ nhớ được chia sẻ có tên
                                                        # SSL，Kích thước khu vực là10MB
        ssl_session_timeout 10m;                        # SSL TCPThời gian chờ của bộ nhớ cache phiên là10phút
        proxy_protocol    on; # bậtproxy_protocol thực tế ip
        proxy_pass        127.0.0.1:1234; # phía sau cuối Trojan cổng nghe
    }
}
```

Vui lòng thêm mã trên vào **http**và**events**hàng giữa

**/etc/nginx/nginx.confTham chiếu tệp cấu hình：**

```text
events {
    worker_connections 768;
    # multi_accept on;
}

stream {
    server {
        listen              443 ssl;                    # Đặt cổng nghe thành443

        ssl_protocols       TLSv1.2 TLSv1.3;      # Đặt phiên bản giao thức SSL được sử dụng

        ssl_certificate /etc/nginx/ssl/xx.com.pem; # Địa chỉ chứng chỉ
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # địa chỉ chính
        ssl_session_cache   shared:SSL:10m;             # SSL TCPBộ đệm phiên đặt vùng bộ nhớ được chia sẻ có tên
                                                        # SSL，Kích thước khu vực là10MB
        ssl_session_timeout 10m;                        # SSL TCPThời gian chờ của bộ nhớ cache phiên là10phút
        proxy_protocol    on; # bậtproxy_protocolthực tếip
        proxy_pass        127.0.0.1:1234; # phía sau cuốiTrojancổng nghe
    }
}

http {

    ##
    # Basic Settings
    ##
```

**Các biện pháp phòng ngừa：**

**1. Hãy cấu hìnhSSLGiấy chứng nhận**

**2. proxy\_pass 127.0.0.1:1234 phía sau cuốiTrojanCổng lắng nghe giống như cổng lắng nghe của nút giao diện người dùng trên trang web của bạn**

**3. listencổng có thể1-65535Vui lòng sửa đổi，Đây là cổng kết nối máy khách**

{% hint style="info" %}
centosVui lòng tắt hệ thốngselinux，Nếu không, việc chuyển tiếp có thể không thành công。

sudo setenforce 0

sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
{% endhint %}

## AikoR Trojancấu hình

**cấu hình chính：**

```text
ListenIP: 127.0.0.1
EnableProxyProtocol: true
EnableFallback: true
CertMode: none
```

{% hint style="info" %}
Để ý1：bảo đảm CertModevìnone，bàn giaoNginxđối phó vớitls
{% endhint %}

{% hint style="info" %}
Để ý2：Khi quay trở lại, hãy đảm bảo rằng trang web dự phòng làhttp1.1，nginxNếu một vị trí là h2, nó sẽ khiến tất cả các vị trí trở thành h2 (hố khổng lồ)
{% endhint %}

**Hoàn thành ví dụ**

```text
  -
    PanelType: "SSpanel" # Panel type: SSpanel, V2board, PMpanel
    ApiConfig:
      ApiHost: "https://xxx.com"
      ApiKey: "123"
      NodeID: 1
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 10 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/AikoR/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 127.0.0.1 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      EnableProxyProtocol: true # Only works for WebSocket and TCP
      EnableFallback: true # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: fake.website.com:80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: none # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/AikoR/cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: /etc/AikoR/cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```

## khởi động lại và kiểm tra Nginx và AikoR

```text
systemctl restart nginx
AikoR restart
```

```text
systemctl status nginx
AikoR status
```

