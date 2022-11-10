# Custom routing function description

AikoRHỗ trợ đầy đủ cho tất cảXray-coreChức năng định tuyến tùy chỉnh được cung cấp được kích hoạt như sau：

1. viết route.jsontài liệu，Cấu hình này giống nhưXray Cấu hình định tuyến hoàn toàn giống nhau, vui lòng kiểm tra：[https://xtls.github.io/config/routing.html](https://xtls.github.io/config/routing.html)Được trợ giúp.
2. hiện hữu`aiko.yml`Cấu hình trung bình`RouteConfigPath`vìroute.jsonmột phần của。
3. Nếu bạn muốn bật cấu hình liên quan đến địa lý, hãy đảm bảo`geoip.dat`và`geosite.dat`trong va`aiko.yml`cùng một thư mục。

{% hint style="info" %}
Được tạo tự động từ các nút có được từ xa inboundTag/outboundTag theo dõi：`NodeType_ListenIP_Port`hình thức. giống：`V2ray_0.0.0.0_80`。Thẻ đến / thẻ đi giống nhau。
{% endhint %}

### Ví dụ về chức năng định tuyến tùy chỉnh

```
  {
    "domainStrategy": "IPOnDemand",
    "rules": [
        {
            "type": "field",
            "outboundTag": "block",
            "ip": [
                "geoip:private"
            ]
        },
        {
            "type": "field",
            "outboundTag": "block",
            "protocol": [
                "bittorrent"
            ]
        },
        {
            "type": "field",
            "outboundTag": "IPv6_out",
            "domain": [
                "geosite:netflix"
            ]
        }
    ]
}
```
