# Hướng dẫn cài đặt

## tải về và sử dụng

1. nơi đây，Chọn phiên bản thích hợp theo hệ thống của bạn：[Release](https://github.com/AikoCute-Offical/AikoR/releases)
2. Giải nén gói nén，chạy sau：`./AikoR -config aiko.yml`

## biên dịch và sử dụng

1. nhiều hơn go 1.19
2. chạy tuần tự

   ```bash
   git clone https://github.com/AikoCute-Offical/AikoR
   cd AikoR/main
   go mod tidy
   go build -o AikoR -ldflags "-s -w"
   ./AikoR -config aiko.yml
   ```

Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](../../Configuration-file-description/config.md)

