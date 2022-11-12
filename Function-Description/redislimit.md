# Mô tả chức năng giới hạn thiết bị ( redislimit )
1. Chức năng này sẽ giới hạn số lượng thiết bị kết nối đến một tài khoản, giúp ngăn chặn các tài khoản được kết nối đến nhiều thiết bị.

## 1.1. Cài đặt Redis

Centos7 or Later

```bash
yum install epel-release -y
yum install redis -y
```

Ubuntu 18.04, Debian 9 or Later

```bash
apt install redis-server -y
```

## 1.2. Cấu Hình Redis

Clear redis configuration file

```bash 
echo "" > /etc/redis.conf
```

Set redis public network to access

```bash
echo "bind 0.0.0.0" >> /etc/redis.conf
```

Set the redis port, it is recommended to set a random port

```bash
echo "port 12345" >> /etc/redis.conf
```

Set the redis password to ensure safety, please set a longer random password

```bash
echo "requirepass 123456" >> /etc/redis.conf
```

Start redis and set up the boot self -starting

```bash
systemctl start redis
systemctl enable redis
```

## 1.3. Cấu Hình AikoR

Vui lòng tham khảo tệp cấu hình: [Mô tả tệp cấu hình](../Configuration-file-description/config.md)

AikoR Hỗ trợ chức năng giới hạn theo tài khoản, vui lòng tham khảo tệp cấu hình để biết chi tiết.hiện hữu`ControllerConfig` Định cấu hình các mục sau trong。

```yaml
RedisConfig:
    RedisLimit: 0 # The Redis limit of a user, 0 means disable
    RedisAddr: 127.0.0.1:6379 # The redis server address format: (IP:Port)
    RedisPassword: PASSWORD # Redis password
    RedisDB: 0 # Redis DB (Redis database number, default 0, no need to change)
    Timeout: 5 # Timeout for Redis request
    Expiry: 60 # Expiry time ( Cache time of online IP, unit: second )
```

| tham số       | Minh họa                                                                    |
| ------------- | --------------------------------------------------------------------------- |
| RedisLimit    | Giới hạn redis của người dùng, 0 có nghĩa là vô hiệu hóa                    |
| RedisAddr     | Định dạng địa chỉ máy chủ Redis: (IP: Cổng)                                 |
| RedisPassword | Mật khẩu của Redis Server                                                   |
| RedisDB       | Redis DB (Số cơ sở dữ liệu Redis, mặc định 0, không cần thay đổi)           |    
| Timeout       | Thời gian chờ cho yêu cầu Redis (Đơn vị: giây)                              |
| Expiry        | Thời gian hết hạn (Thời gian lưu trữ IP trực tuyến, đơn vị: giây)           |

## 2.1. Chức năng Giới hạn thiết bị

AikoR sẽ lưu trữ IP của thiết bị đang kết nối vào Redis, và sẽ xóa IP của thiết bị khi ngắt kết nối. Nếu số lượng thiết bị kết nối đến tài khoản vượt quá giới hạn, AikoR sẽ không cho phép kết nối đến tài khoản.

Nếu RedisLimit sẽ được ưu tiên hơn DeviceLimit trong tệp cấu hình. Nếu RedisLimit = 0, AikoR sẽ sử dụng DeviceLimit.

## 3 Vấn đề thường gặp

### 3.1 Dịch vụ Redis có ngắt kết nối đột ngột không ? Nó sẽ ảnh hưởng đến việc người dùng sử dụng?

Nó sẽ không ảnh hưởng đến việc sử dụng người dùng bình thường, nhưng số giới hạn tăng cường/số kết nối sẽ không thành công và nó sẽ trở lại một giới hạn nút duy nhất cho đến khi dịch vụ Redis có thể được truy cập lại. Quá trình này tự động chuyển đổi mà không cần quan tâm. Những gì bạn cần đảm bảo là dịch vụ Redis có thể truy cập thông thường.

### 3.2 RedisLimit Ý nghĩa cụ thể 

Giá trị này là thời gian của IP/thiết bị trực tuyến bộ đệm, đơn vị: Giây. Giải thích đơn giản: Khi đạt được số lượng/thiết bị của IP của người dùng, khi bạn muốn chuyển đổi IP/thiết bị, nó cần phải chờ khoảng thời gian để chờ (bắt đầu từ IP/thiết bị được kết nối trước đó được tính toán).

Giá trị này không nên được đặt quá nhỏ hoặc quá lớn, vui lòng điều chỉnh theo kinh nghiệm thực tế của bạn.

### 3.3 Tôi có nhiều Panel/nhiều phụ trợ, tôi có cần cài đặt nhiều redis không?

Không, có 16 số cơ sở dữ liệu trong cấu hình mặc định của Redis và số là 0-15, có thể được phân tách.

### 3.4 Tôi có nhiều bảng/nhiều phụ trợ, làm thế nào tôi có thể để chúng hạn chế toàn bộ biên giới cùng nhau?

Bạn có thể sử dụng Redis để hạn chế số lượng thiết bị kết nối đến tài khoản, và sử dụng RedisLimit để hạn chế số lượng thiết bị kết nối đến tài khoản. Nếu bạn muốn tách riêng từng gói thì có thể sử dụng redis DB từ 0-15 để phân tách.