# Docking Trojan

| giao thức | Tình hình hỗ trợ | thỏa thuận hỗ trợ |
| :--- | :--- | :--- |
| Trojan | √ | tcp, grpc |

## Định dạng địa chỉ nút SSpanel-uim

```text
Tên miền hoặc IP;port=Cổng kết nối người dùng # Cổng nghe|host=xx
```

## ví dụ tcp

```text
Thí dụ：gz.aaa.com;port=443|host=gz.aaa.com
```

## ví dụ grpc

Sử dụng trojan + grpc để nâng cấp sspanel lên[Anankke/SSPanel-Uim@8f68b63](https://github.com/Anankke/SSPanel-Uim/commit/8f68b6360baf9f6624e1158e3cae81d93d1db107)

```text
Thí dụ：gz.aaa.com;port=443|host=gz.aaa.com|grpc=1|servicename=mygrpc
```

## Ví dụ về chuyển tuyến

Kết nối người dùng 443，Màn hình AikoR 12345

```text
Thí dụ：gz.aaa.com;port=443#12345|host=hk.aaa.com
```

## kích hoạt xtls **\(Đây là một tính năng thử nghiệm\)**

nâng cấp sspanel lên phiên bản này[Anankke/SSPanel-Uim@8f68b63](https://github.com/Anankke/SSPanel-Uim/commit/8f68b6360baf9f6624e1158e3cae81d93d1db107)Hỗ trợ đăng ký và phân phối xtls sau này

Thêm bất kỳ cấu hình giao thức nào `enable_xtls=true`，Nếu xtls có luồng điều khiển luồng，được thêm vào cuối: `flow=flow-vlaue`

```text
Thí dụ：gz.aaa.com;port=443|host=gz.aaa.com|enable_xtls=true|flow=xtls-rprx-direct
```

Đồng thời thiết lập tệp cục bộ sẽ `EnableXTLS` đặt thành true。 Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](https://github.com/AikoR-project/AikoR-doc/tree/af55d4cc45735ca8d00491aa97f8cbbd97c8faf4/sspanel/config/README.md)

