# Mô tả chức năng nhập tùy chỉnh

AikoRHỗ trợ đầy đủ cho tất cảXray-coreCung cấp chức năng nhập tùy chỉnh，Phương thức kích hoạt cụ thể như sau：

1. viết custom\_inbound.jsontài liệu，Cấu hình này giống như Xray Cấu hình xuất hoàn toàn giống nhau，hãy kiểm tra：[https://xtls.github.io/config/inbound.html](https://xtls.github.io/config/inbound.html)được trợ giúp。
2. hiện hữu`aiko.yml`Cấu hình trung bình`InboundConfigPath`vìcustom\_inbound.jsonmột phần của。

### Ví dụ về chức năng nhập tùy chỉnh

```text
[
    {
        "listen": "0.0.0.0",
        "port": 1234,
        "protocol": "socks",
        "settings": {
            "auth": "noauth",
            "accounts": [
                {
                    "user": "my-username",
                    "pass": "my-password"
                }
            ],
            "udp": false,
            "ip": "127.0.0.1",
            "userLevel": 0
        }
    }
]
```

