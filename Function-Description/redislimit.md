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
