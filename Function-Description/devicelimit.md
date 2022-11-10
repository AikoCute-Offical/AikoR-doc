# Mô tả chức năng hạn chế kết nối thiết bị

Vì một số lượng lớn bảng điều khiển không còn hỗ trợ đặc điểm kỹ thuật giới hạn thiết bị từ xa, thông số giới hạn thiết bị cục bộ hiện đã được thêm vào.

Để kích hoạt, nó có thể được đặt trong tệp cấu hình`DeviceLimit`Đặt thành giá trị khác 0, lưu ý rằng cài đặt này sẽ ghi đè số lượng thiết bị người dùng truy cập từ xa。

Vui lòng tham khảo tệp cấu hình: [Mô tả tệp cấu hình](../Configuration-file-description/config.md)

{% hint style="info" %}
Mỗi địa chỉ IP duy nhất được coi là một thiết bị.
{% endhint %}

## Giới hạn thiết bị toàn cầu

khi nào AikoR Phiên bản &gt;=v0.7.1，SSpanel Phiên bản &gt;=[2021.9](https://github.com/Anankke/SSPanel-Uim/releases/tag/2021.9)，AikoR Tính năng hạn chế thiết bị toàn cầu sẽ được bật cho SSpanel. Tại thời điểm này, các nút phụ trợ khác nhau sẽ giới hạn toàn cầu số lượng kết nối IP độc lập, thay vì giới hạn cục bộ của mỗi phần phụ trợ.

Khi giới hạn thiết bị là 1, việc chuyển đổi giữa các nút khác nhau sẽ bị hạn chế. Bạn nên đặt số lượng thiết bị ít nhất là 2. Và do giới hạn bảng điều khiển SSPanel, có thể mất ít nhất 2 phút để thông tin kết nối IP được truyền đến tất cả các nút phụ trợ, vì vậy các kết nối đồng thời trong vòng 2 phút sẽ không bị hạn chế.