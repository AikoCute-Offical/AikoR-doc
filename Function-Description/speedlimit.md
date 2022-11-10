# Mô tả chức năng giới hạn tốc độ

1. Giới hạn tốc độ nút: Vui lòng điền vào giới hạn tốc độ nút của SSpanel, tính bằng Mbps.
2. Giới hạn tốc độ người dùng: Vui lòng điền vào cài đặt người dùng của SSpanel, đơn vị là Mbps.
3. Nếu giá trị giới hạn tốc độ được đặt thành 0, có nghĩa là không có giới hạn tốc độ.


## Cài đặt giới hạn tốc độ nút cục bộ
Đối với bảng không hỗ trợ cài đặt giới hạn tốc độ từ xa: chẳng hạn như V2board, giới hạn tốc độ có thể được đặt trong tệp cấu hình cục bộ `SpeedLimit`. Lưu ý rằng cài đặt này ghi đè giới hạn tốc độ cấp nút để tìm nạp từ xa.

{% hint style="info" %}
Giới hạn tốc độ nút: Tất cả các giá trị giới hạn tốc độ của người dùng được kết nối với nút này sẽ nhận giá trị được đặt trong `SpeedLimit` ** (không phải giới hạn tốc độ cổng)**
{% endhint %}

Vui lòng tham khảo tệp cấu hình: [Mô tả tệp cấu hình](../Configuration-file-description/config.md)

## Chức năng giới hạn tốc độ động

AikoR Hỗ trợ chức năng giới hạn tốc độ động, giá trị giới hạn tốc độ có thể được điều chỉnh theo lưu lượng sử dụng của người dùng. hiện hữu`ControllerConfig` Định cấu hình các mục sau trong。

{% hint style="info" %}
Để đảm bảo có thể kịp thời điều chỉnh giới hạn tốc độ cho các kết nối dài (chẳng hạn như tải xuống dài), hãy đảm bảo rằng giá trị giới hạn tốc độ mặc định lớn hơn 0. Giá trị giới hạn tốc độ mặc định lớn hơn 0 có thể được định cấu hình tại bất kỳ vị trí nào của người dùng, nút hoặc cục bộ và ưu tiên là cục bộ> nút> người dùng. Nếu giá trị giới hạn tốc độ mặc định là 0, giới hạn tốc độ động của người dùng sẽ chỉ có hiệu lực vào lần thiết lập kết nối tiếp theo.
{% endhint %}


```yaml
AutoSpeedLimitConfig:
    Limit: 0 # Warned speed. Set to 0 to disable AutoSpeedLimit (mbps)
    WarnTimes: 0 # After (WarnTimes) consecutive warnings, the user will be limited. Set to 0 to punish overspeed user immediately.
    LimitSpeed: 0 # The speedlimit of a limited user (unit: mbps)
    LimitDuration: 0 # How many minutes will the limiting last (unit: minute)
```
| tham số         | Minh họa                                                                        |
| ------------- | --------------------------------------------------------------------------- |
| Limit         | Cảnh báo tốc độ. Đặt thành 0 để tắt giới hạn tốc độ tự động. đơn vị：Mbps。                           |
| WarnTimes     | Số lượng cảnh báo. Sau khi số lượng cảnh báo liên tiếp đạt đến giá trị này, người dùng sẽ bị điều chỉnh. Đặt thành 0 để ngay lập tức trừng phạt người dùng chạy quá tốc độ。 |
| LimitSpeed    | giá trị giới hạn tốc độ. Đơn vị: Mbps.                                                        |
| LimitDuration |Thời gian giới hạn tốc độ. Đơn vị: phút.                                                      |