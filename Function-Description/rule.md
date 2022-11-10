# Mô tả chức năng kiểm tra

1. Vui lòng điền vào bất kỳ biểu thức chính quy nào trong quy tắc kiểm tra giao diện người dùng，giống `baidu.com`Tất cả các miền baidu sẽ bị chặn，`(.+\.|^)(360|so)\.(cn|com)`sẽ chặn360Các trang web liên quan。
2. hỗ trợ đầu vàoip che địa chỉ ip，giống`127.0.0.1`。
3. BTChặn giao thức vui lòng xem：[Mô tả chức năng định tuyến tùy chỉnh](zi-ding-yi-lu-you-gong-neng-shuo-ming.md)

## Cài đặt quy tắc kiểm tra cục bộ

Đối với các bảng không hỗ trợ thiết lập từ xa các quy tắc kiểm tra：giốngV2board，có thể được cấu hình cục bộ`RuleListPath`Đặt đường dẫn tệp quy tắc cục bộ。Tệp quy tắc không cần xác định loại tệp，mỗi**quy tắc thông thường**một đường thẳng，Nhãn ID quy tắc cục bộ mặc định là-1。

Xem tệp cấu hình để biết chi tiết：[Mô tả tệp cấu hình](../Configuration-file-description/config.md)

**Ví dụ về tệp quy tắc cục bộ**

Hãy đảm bảo rằng mỗi dòng chỉ là một quy tắc thông thường đơn giản, không chứa bất kỳ chuỗi nào khác không liên quan。

```text
(.+\.|^)(360|so)\.(cn|com)
baidu.com
google.com
127.0.0.1
```

