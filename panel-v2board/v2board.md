# Cấu hình đế cắm cơ bản

1. hiện hữu `aiko.yml` Cấu hình trung bình `PanelType: "V2board"`。
2. V2board Chỉ loại nút V2ray hỗ trợ các quy tắc kiểm tra cấu hình bảng điều khiển，Đối với các giao thức khác, vui lòng sử dụng AikoR[Chức năng kiểm toán địa phương](../Function-Description/rule.md)。
3. kích hoạt vless và xtls，Vui lòng bắt đầu theo cách thủ công trong tệp cấu hình，V2board không hỗ trợ cấu hình trực tuyến，Đồng thời, V2board không hỗ trợ phân phối vless và xtls，Vui lòng sửa đổi cấu hình máy khách theo cách thủ công，Hoặc tự tìm các giải pháp khác。

Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](../Configuration-file-description/config.md)

### Gắn vmess + ws
v2board cần thêm phần sau vào cấu hình giao thức truyền tải，Định cấu hình đường dẫn của ws：
```
{
  "path": "/name",
}
```
Trong`"name"`thay thế bằng bất kỳ chuỗi nào，Có thể được sử dụng để tạo shunting ngược như nginx。

### Gắn vmess + ws + tls
v2board cần thêm phần sau vào cấu hình giao thức truyền tải，Định cấu hình đường dẫn của ws và tên miền của tls：
```
{
  "path": "/",
  "headers": {
    "Host": "v2ray.com"
  }
}
```
Trong`"name"`thay thế bằng bất kỳ chuỗi nào，Có thể được sử dụng để tạo shunting ngược như nginx，`"Host"`Thay đổi tên miền sau thành tên miền giả của riêng bạn。

### Gắn kết vmess + grpc

Để hỗ trợ thành công kết nối sự cố，Khi gắn vmess + grpc，v2board cần thêm nội dung sau vào cấu hình giao thức truyền：

```text
{
  "serviceName": "name",
}
```

Trong`"name"`thay thế bằng bất kỳ chuỗi nào，Nó có thể được sử dụng để tạo shunting ngược như nginx.

### Gắn vmess + tcp + http

{% hint style="info" %}
Bản V2board không hỗ trợ phân phối đăng ký tcp + http，Hãy tự mình tìm ra giải pháp，hoặc định cấu hình thủ công tệp khách hàng。
{% endhint %}

Khi gắn vmess + tcp + http，v2board cần thêm nội dung sau vào cấu hình giao thức truyền：

```text
{
  "header": {
    "type": "http",
    "request": {},
    "response": {}
  }
}
```

Trong`request`và`response`Vui lòng tham khảo nội dung trong[Tài liệu Xray-core](https://xtls.github.io/config/transports/tcp.html#httpheaderobject)cài đặt.

