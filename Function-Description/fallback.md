# Fallback Mô tả chức năng

> fallback vì XrayCung cấp khả năng phát hiện chống chủ động cường độ cao, Và có cơ chế dự phòng gói đầu tiên ban đầu.
>
> fallback Các loại lưu lượng truy cập khác nhau cũng có thể path chuyển hướng, do đó thực hiện một cổng, Chia sẻ nhiều dịch vụ.
>
> Hiện tại bạn có thể sử dụng VLESS hoặc trojan hiệp định, theo cấu hình fallbacks để sử dụng tính năng dự phòng, Và tạo ra sự kết hợp rất phong phú của lối chơi.
>
> ---[https://xtls.github.io/config/features/fallback.html](https://xtls.github.io/config/features/fallback.html)

## cho phépFallbackHàm số

cài đặt`EnableFallback`vì`true`，và cấu hình`FallBackConfigs`

```yaml
ControllerConfig:
  EnableFallback: true # Only support for Trojan and Vless
  FallBackConfigs:  # Support multiple fallbacks
    -
      SNI: # TLS SNI(Server Name Indication), Empty for any
      Alpn: # Alpn, Empty for any
      Path: # HTTP PATH, Empty for any
      Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
      ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
```

## 配置Fallback

AikoRtheo dõiXrayÝ tưởng thiết kế，Hỗ trợ một nút nhiềuFallbackcài đặt，vì thế`FallBackConfigs`như một mảng，Ví dụ về mỗi phần tử con như sau：

```yaml
-
  SNI: # TLS SNI(Server Name Indication), Empty for any
  Alpn: # Alpn, Empty for any
  Path: # HTTP PATH, Empty for any
  Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
  ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
```

### SNI: string

cố gắng kết hợp TLS SNI\(Server Name Indication\)，trống cho bất kỳ，Mặc định là ""

### Alpn: string
cố gắng kết hợp TLS ALPN Kết quả đàm phán，trống cho bất kỳ，Mặc định là ""

khi cần，VLESS sẽ cố gắng đọc TLS ALPN Kết quả đàm phán，nếu thành công，đầu ra info `realAlpn =` để đăng nhập。
Sử dụng: đã giải quyết Nginx của h2c Các dịch vụ không tương thích đồng thời http/1.1 Vấn đề，Nginx cần viết hai dòng listen，được sử dụng cho 1.1 và h2c。
Để ý：fallbacks alpn hiện hữu `"h2"` Thời gian，[Inbound TLS](../transport.md#tlsobject) cần phải được thiết lập `"alpn":["h2","http/1.1"]`，để hỗ trợ truy cập h2。

{% hint style="info" %}
Fallback đặt trong `alpn` là để phù hợp với thương lượng thực tế ALPN，và Inbound TLS bộ `alpn` là danh sách các ALPN tùy chọn trong quá trình bắt tay，Cả hai đều có ý nghĩa khác nhau。
{% endhint %}

### Path: string

cố gắng khớp với gói đầu tiên HTTP PATH，trống cho bất kỳ，Mặc định trống，không trống phải kết thúc bằng "/" bắt đầu，không hỗ trợ h2c。

Thông minh: khi cần thiết，VLESS sẽ cố gắng để có một cái nhìn PATH（Không nhiều hơn 55 byte；thuật toán nhanh nhất，phân tích không đầy đủ HTTP），nếu thành công，đầu ra info realPath = để đăng nhập。 Sử dụng: chuyển hướng khác inbound của WebSocket giao thông hoặc HTTP Giả mạo giao thông，không cần xử lý thêm、Chuyển tiếp hoàn toàn lưu lượng truy cập，Tỷ lệ đo lường Nginx Chống thế hệ mạnh mẽ hơn。

Để ý：fallbacks nơi mà bản thân nó phải ở TCP+TLS，Cái này được chuyển hướng sang cái khác WS cho đến，Chuyển hướng đến không cần phải định cấu hình TLS。

### Dest: string\|number

Quyết định TLS sau khi giải mã TCP giao thông đang đi đến đâu，Hiện hỗ trợ hai loại địa chỉ：（Trường này là bắt buộc，Nếu không nó sẽ không bắt đầu）

1. TCP，Định dạng là "addr:port"，Trong addr ủng hộ IPv4、tên miền、IPv6，Nếu bạn điền vào tên miền，cũng sẽ trực tiếp bắt đầu TCPliên kết（thay vì đi bộ cài sẵn DNS）。
2. Unix domain socket，định dạng là đường dẫn tuyệt đối，Có hình dạng như "/dev/shm/domain.socket"，có thể được thêm vào đầu "@" đại diện abstract，"@@" đại diện cho thắt lưng padding của abstract。

   Nếu chỉ điền vào port，Số hoặc chuỗi có thể là，Có hình dạng như 80、"80"，thường trỏ đến một bản rõ http Phục vụ（addr sẽ được bổ sung như "127.0.0.1"）。

### ProxyProtocolVer: number

gửi PROXY protocol，Một nguồn sự thật dành riêng cho việc cung cấp các yêu cầu IP và cổng, điền vào phiên bản 1 hoặc 2, mặc định là 0, nghĩa là không được gửi. Điền vào 1 nếu cần thiết.

Hiện tại điền 1 hoặc 2, chức năng hoàn toàn giống nhau, nhưng cấu trúc khác nhau, và cái trước có thể được in ra, cái sau là nhị phân.Xraycủa TCP và WS Inbound đã được hỗ trợ để nhận PROXY protocol。

> TIP
>
> Nếu bạn đang định cấu hình Nginx Đảm nhận PROXY protocol，Ngoại trừ thiết lập proxy\_protocol ngoài，Vẫn cần đặt set\_real\_ip\_from，Nếu không có thể có vấn đề。

## Fallback Thí dụ

AikoRcài đặt

```text
EnableFallback: true
FallBackConfigs:  # Support multiple fallbacks
  -
    SNI:
    Alpn:
    Path:
    Dest: 8080
    ProxyProtocolVer: 0
```

Nginx cài đặt

```text
server {  
    listen 8080 http2;
  root /var/www/public; # thay đổi con đường của riêng bạn
  index index.php index.html;
  server_name www.test.com; # Thay đổi thành tên miền của riêng bạn

  location / {
    try_files $uri /index.php$is_args$args;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass 127.0.0.1:9000; # unix:/run/php/php-fpm.sock;
  }
}
```

## tham khảo

[Xray Fallback](https://xtls.github.io/config/features/fallback.html)

