# Custom export function description

AikoR Hỗ trợ đầy đủ cho tất cả Xray-core Cung cấp chức năng xuất tùy chỉnh，Phương thức kích hoạt cụ thể như sau：

1. viết custom\_outbound.json tài liệu，Cấu hình này giống như Xray Cấu hình xuất hoàn toàn giống nhau, vui lòng xem：[https://xtls.github.io/config/outbound.html](https://xtls.github.io/config/outbound.html)được trợ giúp。
2. hiện hữu`aiko.yml`Cấu hình trung bình`OutboundConfigPath`vìcustom\_outbound.jsonmột phần của。

### Ví dụ về chức năng xuất tùy chỉnh

```
[
    {
        "tag": "IPv4_out",
        "protocol": "freedom"
    },
    {
        "tag": "IPv6_out",
        "protocol": "freedom",
        "settings": {
            "domainStrategy": "UseIPv6"
        }
    },
    {
        "protocol": "blackhole",
        "tag": "block"
    }
]
```
