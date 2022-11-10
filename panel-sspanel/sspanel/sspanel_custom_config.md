# Cập nhật phiên bản mới SSPanel Custom Config

cho sspanel >= Phương pháp cấu hình để tự động bật Custom_config trong phiên bản 2021.11，Vui lòng xem cấu hình sau，Định cấu hình chính xác thông tin nút。Thông tin về đăng ký，hãy kiểm tra[Tài liệu liên quan đến SSPanel](https://wiki.sspanel.org/#/universal-subscription)。
Nếu bạn không muốn sử dụng custom config，xin vui lòng tại `ApiConfig` Trung tướng `DisableCustomConfig` thiết lập như `true`。

# Shadowsocks
```json
{
    "offset_port_user": "12345", //Cổng được phát hành trong giao diện người dùng / đăng ký
    "offset_port_node": "12345", //Cổng do máy chủ nút cấp
    "server_user": "hk.domain.com", //Địa chỉ máy chủ được phân phối trong giao diện người dùng / đăng ký
    "mu_encryption": "chacha20-ietf-poly1305", // `aes-128-gcm`, `aes-256-gcm`, `chacha20-ietf-poly1305`một trong ba
}

```

# V2ray

AlterId được đặt thành 0，sau đó tự động bật VMessAEAD。

{% hint style="info" %} Để ý：VMESS AEAD sẽ là bắt buộc vào ngày 1 tháng 1 năm 2022 Xin lưu ý để cập nhật cấu hình máy chủ，đặt AlterId = 0 {% endhint %}

## ví dụ tcp

``` json
{
	"offset_port_node": 12345,
	"server_sub": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "none",
}
```

## ví dụ tcp + http

```json
{
	"offset_port_node": 12345,
	"server_sub": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "none",
	"header": {
        "type": "http",
        "request": {
            "path": ["/"],
  			"headers": {
    			"Host": ["www.baidu.com"]
            }
        },
        "response": {}
    }
}
```

## ví dụ tcp + tls

```json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "tls",
}
```

## ví dụ ws

```json
{
	"offset_port_node": 80,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "ws",
	"security": "none",
	"path": "/v2ray"
}
```

## ví dụ ws + tls

```json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "ws",
	"security": "tls",
	"path": "/v2ray"
}
```

## ví dụ grpc + tls

```json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "grpc",
	"security": "tls",
	"servicename": "some_name"
}
```

## Ví dụ về cảng chuyển tuyến
đặt trong một trong hai cấu hình `offset_port_user` Kết nối cổng cho người dùng

``` json
{
	"offset_port_user": 8888,
	"offset_port_node": 12345,
	"server_sub": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "none",
}
```

Tại thời điểm này, cổng kết nối người dùng là 8888 và cổng lắng nghe của nút là 12345.

## cho phép vless
đặt trong một trong hai cấu hình `enable_vless: 1` Kết nối cổng cho người dùng

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "tls",
	"enable_vless": 1
}
```
Vui lòng bật vless và đảm bảo sử dụng tls hoặc xtls。

## kích hoạt xtls
đặt trong một trong hai cấu hình `security: xtls`。

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"alter_id": 0,
	"network": "tcp",
	"security": "xtls",
	"enable_vless": 1
}
```

# Trojan

## ví dụ tcp

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com"
}
```

## ví dụ grpc

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"grpc": 1,
	"servicename": "some_name"
}
```

## Ví dụ về Trojan-go ws

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"network": "ws",
	"path": "/path"
}
```

## Ví dụ về chuyển tuyến
đặt trong một trong hai cấu hình `offset_port_user` Kết nối cổng cho người dùng
``` json
{
	"offset_port_user": 443,
	"offset_port_node": 12345,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com"
}
```
Tại thời điểm này, người dùng kết nối 443 và nút lắng nghe 12345

## kích hoạt xtls

đặt trong một trong hai cấu hình `enable_xtls: 1`。

``` json
{
	"offset_port_node": 443,
	"server_sub": "hk.domain.com",
	"host": "hk.domain.com",
	"enable_xtls": 1
}
```
