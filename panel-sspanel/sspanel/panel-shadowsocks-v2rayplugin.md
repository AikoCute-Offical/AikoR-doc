# Shadowsocks - V2Ray-Plugin

| Giao Thức                  | Phương pháp mã hóa                               | Method of confusion                                      |
| -------------------------- | ------------------------------------------------ | -------------------------------------------------------- |
| Shadowsocks - V2Ray-Plugin | aes-128-gcm, aes-256-gcm, chacha20-ietf-poly1305 | <p>simple_obfs_http,simple_obfs_tls,</p><p>ws,ws+tls</p> |

## Định dạng địa chỉ nút SSpanel-uim

```
IP;cổng nghe;;(ws hoặc obfs);(tls hoặc để trống);path=/xxx|host=xxxx.com|server=xxx.com|outside_port=xxx
```

Lưu ý hai dấu chấm phẩy sau cổng nghe

## Sửa đổi mã SSpanel-uim

SSpanel-uim về Shadowsocks - Có một số vấn đề với mã của V2Ray-Plugin，Cần phải sửa đổi để cấp đăng ký một cách chính xác。

Phương pháp này được viết bằng [SSPanel-Uim@822d3c](https://github.com/Anankke/SSPanel-Uim/commit/822d3cbcb3ad8f7e11874a96f05d73e5b016c164)，Không có gì đảm bảo rằng nó sẽ vẫn có hiệu quả trong tương lai。

### Phương pháp sửa đổi

Mở tệp src \ Models \ Node.php，tìm dòng 420，chú thích nó。

trước khi sửa chữa：

```
$return_array['path'] = ($return_array['path'] . '?redirect=' . $user->getMuMd5());
```

sau khi sửa đổi：

```
// $return_array['path'] = ($return_array['path'] . '?redirect=' . $user->getMuMd5());
```

## Đăng ký SSpanel-uim

SSpanel-uim đề xuất Android，WIN và Mac sử dụng Clash，IOS sử dụng Shadowrocket để nhận Shadowsocks - V2Ray-Đăng ký plugin。

## ví dụ ws + tls (Nginx )（**giới thiệu**）

Để Caddy hoặc Nginx xử lý cấu hình nút TLS và ws + tls，Định cấu hình trên chương trình phụ trợ `CertMode: none`

Cũng được đặt bên ngoài \ \_port làm cổng nghe Nginx，Chuyển tiếp đến 12345 cho cổng nghe AikoR。Có thể được định cấu hình trong phần phụ trợ `ListenIP: 127.0.0.1` nghe trên cổng địa phương。

```
ip;12345;;ws;tls;path=/xxx|server=tên miền|host=tên miền CDN|outside_port=443
```

```
Thí dụ：1.3.5.7;12345;;ws;tls;path=/ss|server=hk.domain.com|host=hk.domain.com|outside_port=443
```

## ví dụ ws + tls

```
ip;12345;;ws;tls;path=/xxx|host=xxxx.com|server=xxx.com
```

```
Thí dụ：1.3.5.7;12345;;ws;tls;path=/ss|host=hk.domain.com|server=hk.domain.com
```

## ví dụ ws

```
ip;12345;;ws;;path=/xxx|host=xxxx.com|server=xxx.com
```

```
Thí dụ：1.3.5.7;12345;;ws;;path=/ss|host=hk.domain.com|server=hk.domain.com
```

## ví dụ đơn giản \_obfs \_http

```
ip;12345;;obfs;http;server=xxx.com
```

```
Thí dụ：1.3.5.7;12345;;obfs;http;server=hk.domain.com
```

## ví dụ về simple \_obfs \_tls

```
ip;12345;;obfs;tls;server=xxx.com
```

```
Thí dụ：1.3.5.7;12345;;obfs;tls;server=hk.domain.com
```

## Cảng quá cảnh

Sự gia tăng sau bất kỳ kết hợp cấu hình nào `|outside_port=xxx`,Đây là cổng kết nối người dùng。

AikoR không `inside_port=xx` Tùy chọn cấu hình，Để nghe trên một cổng cục bộ，Vui lòng đặt ip nghe trong tệp cấu hình là `127.0.0.1`。

```
Thí dụ：1.3.5.7;12345;;ws;tls;path=/ss|server=hk.domain.com|host=hk.domain.com|outside_port=8888
```
