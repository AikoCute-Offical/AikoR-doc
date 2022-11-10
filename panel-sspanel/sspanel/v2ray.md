# Gắn kết với V2ray

| giao thức | Tình hình hỗ trợ |
| :--- | :--- |
| VMess | tcp, tcp+http, tcp+tls, ws, ws+tls, h2c, h2+tls, grpc, grpc+tls |
| VMessAEAD | tcp, tcp+http, tcp+tls, ws, ws+tls, h2c, h2+tls, grpc, grpc+tls |
| VLess | tcp, tcp+http, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls, grpc, grpc+tls/xtls |

## Định dạng địa chỉ nút SSpanel-uim

```text
IP;cổng nghe;alterId;(tcp hoặc ws);(tls hoặc để trống);path=/xxx|host=xxxx.com|server=xxx.com|outside_port=xxx
```

AlterId được đặt thành 0，sau đó tự động bật VMessAEAD。

{% hint style="info" %} Để ý：VMESS AEAD sẽ là bắt buộc vào ngày 1 tháng 1 năm 2022 Xin lưu ý để cập nhật cấu hình máy chủ，đặt AlterId = 0 {% endhint %}

## ví dụ tcp

```text
ip; 12345; 0; tcp; server=tên miền
```

```text
Thí dụ：1.3.5.7; 12345; 0; tcp; server=hk.domain.com
```

## ví dụ tcp + http

Lưu ý rằng sspanel không hỗ trợ phân phối đăng ký như vậy，Tùy chọn này chỉ để kích hoạt tính năng giải mã http phụ trợ。

```text
ip; 12345; 0; tcp; server=tên miền; headertype=http
```

```text
Thí dụ：1.3.5.7; 12345; 0; tcp; server=hk.domain.com;headertype=http
```

## Thí dụ tcp + tls 

```text
ip; 12345; 0; tcp; tls; server=tên miền|host=tên miền
```

```text
Thí dụ：1.3.5.7;12345;0;tcp;tls;server=hk.domain.com|host=hk.domain.com
```

## Thí dụ ws

```text
ip; 80; 0; ws; path=/xxx|server=tên miền|host=Tên miền CDN
```

```text
Thí dụ：1.3.5.7; 80; 0; ws; path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## ví dụ ws + tls

```text
ip;443;0;ws;tls;path=/xxx|server=tên miền|host=tên miền CDN
```

```text
Thí dụ：1.3.5.7;443;0;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## ví dụ ws + tls \(Caddy / Nginx \)

Để Caddy hoặc Nginx xử lý cấu hình nút TLS và ws + tls，Định cấu hình trên chương trình phụ trợ `CertMode: none`

Cũng được đặt bên ngoài \ _port làm cổng nghe Caddy / Nginx，Chuyển tiếp đến 12345 cho cổng nghe AikoR。Có thể được định cấu hình trong phần phụ trợ `ListenIP: 127.0.0.1` nghe trên cổng địa phương。

```text
ip;12345;0;tls;ws;path=/xxx|server=tên miền|host=tên miền CDN|outside_port=443
```

```text
Thí dụ：1.3.5.7;12345;0;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
Thí dụ：1.3.5.7;12345;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## ví dụ grpc + tls

Sử dụng grpc khuyên bạn nên nâng cấp sspanel lên[Anankke/SSPanel-Uim@8f68b63](https://github.com/Anankke/SSPanel-Uim/commit/8f68b6360baf9f6624e1158e3cae81d93d1db107)

```text
ip;12345;0;grpc;tls;host=tên miền|server=tên miền|servicename=bất kỳ chuỗi nào
```

```text
Thí dụ：1.3.5.7;12345;0;grpc;tls;host=hk.domain.com|server=hk.domain.com|servicename=mygrpc
```

## Cảng quá cảnh

trong bất kỳ nhóm cấu hình nào \|tăng sau`|outside_port=xxx`,Đây là cổng kết nối người dùng。

AikoR không `inside_port=xx` Tùy chọn cấu hình，Để nghe trên một cổng cục bộ，Vui lòng đặt ip nghe trong tệp cấu hình là `127.0.0.1`。

```text
Thí dụ：1.3.5.7;80;0;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com|outside_port=12345
```

## Bật Vless

Đây là một tính năng thử nghiệm，Hãy đảm bảo rằng bảng điều khiển bạn đang sử dụng đã hỗ trợ đăng ký vless，Nếu không, hãy định cấu hình máy khách theo cách thủ công。

nâng cấp sspanel lên phiên bản này[Anankke/SSPanel-Uim@8f68b63](https://github.com/Anankke/SSPanel-Uim/commit/8f68b6360baf9f6624e1158e3cae81d93d1db107)Hỗ trợ sau đó không cần đăng ký và giao hàng

Được thêm vào sau bất kỳ cấu hình giao thức nào `enable_vless=true`

```text
Thí dụ：hk.domain.com;12345;0;tcp;(tls或xtls);server=hk.domain.com|enable_vless=true
```

Đồng thời thiết lập tệp cục bộ sẽ `EnableVless` đặt thành true。 Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](../../Configuration-file-description/config.md#mian-ban-dui-jie-pei-zhi)

Vui lòng bật vless và đảm bảo sử dụng tls hoặc xtls。

## kích hoạt xtls

Đây là một tính năng thử nghiệm，Hãy đảm bảo rằng bảng điều khiển bạn đang sử dụng đã hỗ trợ đăng ký bằng xtls，Nếu không, hãy cấu hình máy khách theo cách thủ công.

nâng cấp sspanel lên phiên bản này[Anankke/SSPanel-Uim@8f68b63](https://github.com/Anankke/SSPanel-Uim/commit/8f68b6360baf9f6624e1158e3cae81d93d1db107)Hỗ trợ đăng ký và phân phối xtls sau này

Đặt bất kỳ cấu hình giao thức nào vào `tls` thay thế bằng `xtls`，Nếu xtls có luồng điều khiển luồng，được thêm vào cuối: `|flow=flow-vlaue`

```text
Thí dụ：hk.domain.com;443;0;tcp;xtls;server=hk.domain.com|host=hk.domain.com|enable_vless=true|flow=xtls-rprx-direct
```

Đồng thời thiết lập tệp cục bộ sẽ `EnableXTLS` đặt thành true。 Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](../../Configuration-file-description/config.md#mian-ban-dui-jie-pei-zhi)

