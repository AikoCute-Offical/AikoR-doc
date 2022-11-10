# Tối ưu hóa bộ nhớ liên quan

## Tối ưu hóa kiểm soát liên kết

bằng cách tùy chỉnhConnetionConfig`kết nối được phát hành[Cấu hình liên quan](../Configuration-file-description/config.md)，Có thể tối ưu hóa việc sử dụng bộ nhớ ở một mức độ nhất định

1. giảm`ConnIdle`Có thể tối ưu hóa dung lượng bộ nhớ để có số lượng kết nối cao，Nhưng nó sẽ khiến độ trễ kết nối của người dùng tăng lên。
2. hiện hữu HTTP Duyệt qua cảnh，có thể `UplinkOnly` và `DownlinkOnly` thiết lập như 0，Để cải thiện hiệu quả của việc đóng kết nối，Giảm mức sử dụng bộ nhớ。
3. giảm`BufferSize`Sử dụng bộ nhớ có thể được tối ưu hóa，Nhưng nó có thể khiến việc sử dụng CPU tăng lên。

