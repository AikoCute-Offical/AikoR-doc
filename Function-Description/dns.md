# Mô tả DNS tùy chỉnh

AikoRHỗ trợ thiết lập các chính sách DNS khác nhau cho các nút khác nhau，Phương pháp cụ thể như sau：

1. viết dns.json tài liệu，Cấu hình này giống như Xray DNSCấu hình hoàn toàn giống nhau, vui lòng kiểm tra：[https://xtls.github.io/config/dns.html](https://xtls.github.io/config/dns.html) Được trợ giúp.
2. hiện hữu`aiko.yml`Cấu hình trung bình`DnsConfigPath`đường dẫn đến dns.json。
3. Trong nút cần bật DNS tùy chỉnh, hãy đặt`EnableDNS`thiết lập nhưtrue。Nếu được đặt thànhfalseHoặc để trống, sử dụng máyDNS。
4. Nếu bạn muốn kích hoạt geoipcấu hình liên quan, vui lòng đảm bảo`geoip.dat`và`geosite.dat`trong va`aiko.yml`cùng một thư mục。

## DNSMở khóa cấu hình mẫu

```javascript
{
    "servers": [
      "8.8.8.8", 
      {
        "address": "1.1.2.2", // đã mua DNS Mở khóa được cung cấp IP
        "port": 53,
        "domains": [
          "geosite:netflix" 
        ]
      }
    ]
  }
```

## Đặt mức độ ưu tiên IPV6

1. Đảm bảo máy chủ lưu trữ có địa chỉ ipv6 trước，Nếu không, vui lòng xem xét sử dụng[warp](https://github.com/P3TERX/warp.sh)Đượcipv6。
2. Trong nút cần đặt ưu tiên IPV6，Sẽ`EnableDNS`thiết lập như true。
3. Trong nút cần đặt ưu tiên IPV6，Sẽ`SendIP`thiết lập như`"::"`。
4. Trong nút cần đặt ưu tiên IPV6，Sẽ`DNSType`thiết lập như`UseIP`。

cho đến nay，AikoRĐịa chỉ ipv6 của trang web đích sẽ được sử dụng ưu tiên để truy cập và sẽ không ảnh hưởng đến quyền truy cập của trang ipv4 mặc định。~~Có thể được sử dụng để bỏ chặn Netflix và các nhu cầu khác~~

## Đặt mức độ ưu tiên IPV4

1. Trong nút cần đặt ưu tiên IPV4，Sẽ`EnableDNS`thiết lập nhưtrue。
2.Trong nút cần đặt ưu tiên IPV4，Sẽ`SendIP`thiết lập như`"0.0.0.0"`。
3.Trong nút cần đặt ưu tiên IPV4，Sẽ`DNSType`thiết lập như`UseIP`。

