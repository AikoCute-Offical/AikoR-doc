# Docking Shadowsocks

| giao thức | Tình hình hỗ trợ | phương pháp mã hóa |
| :--- | :--- | :--- |
| ShadowsocksAEAD | √ | aes-128-gcm, aes-256-gcm, chacha20-ietf-poly1305 |

## Định dạng địa chỉ nút SSpanel-uim

* thận trọng，Vui lòng chọn loại nút：`Shadowsocks`
* Phương pháp mã hóa người dùng mang nhiều người dùng một cổng, vui lòng chọn：`aes-128-gcm`, `aes-256-gcm`, `chacha20-ietf-poly1305`một trong ba.
* AikoR hiện chỉ hỗ trợ một người dùng mang đa người dùng một cổng，Chỉ cái đầu tiên được sử dụng khi có nhiều người dùng được lưu trữ.

  ```text
  Tên miền hoặc IP;port=cổng nghe # cổng kết nối;server=xx
  ```

## Ví dụ về Shadowsocks

```text
Thí dụ：gz.aaa.com;port=80#1234;server=gz.aaa.com
```

