# Hướng dẫn đăng ký chứng chỉ tự động

AikoR hỗ trợ nhiều cấu hình ứng dụng chứng chỉ tự động。Chứng chỉ đã áp dụng sẽ được đặt trong**tập tin cấu hình\(aiko.yml\)danh mục`cert`dưới thư mục**。

Sau đây là các mô tả tệp cấu hình có liên quan để tự động đăng ký chứng chỉ.

```yaml
CertConfig:
    CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
    CertDomain: "node2.test.com" # Domain to cert
    CertFile: /etc/AikoR/cert/node2.test.com.cert # Provided if the CertMode is file
    KeyFile: /etc/AikoR/cert/node2.test.com.key
    Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
    Email: test@me.com
    DNSEnv: # DNS ENV option used by DNS provider
        ALICLOUD_ACCESS_KEY: aaa
        ALICLOUD_SECRET_KEY: bbb
```

| tham số | Tùy chọn | Minh họa |
| :--- | :--- | :--- |
| `CertMode` | `none`,`file`,`http`,`dns` | Cách lấy chứng chỉ。`file`:Cung cấp thủ công，và tạo một con đường。`http`：Đăng ký qua http，yêu cầu cổng 80。`dns`：Áp dụng bằng chế độ dns，Cấu hình nhà cung cấp dịch vụ dns có liên quan cần được xây dựng。`none`：buộc đóng cài đặt tls，Giao nó cho nginx hoặc caddy。 |
| `CertDomain` | không ai | Đăng ký tên miền chứng chỉ |
| `CertFile` | không ai | Đường dẫn chứng chỉ được chỉ định theo cách thủ công |
| `KeyFile` | không ai | Đường dẫn khóa cá nhân được chỉ định theo cách thủ công |
| `Provider` | không ai | nhà cung cấp dns，Tất cả các nhà cung cấp dns được hỗ trợ có thể được tìm thấy tại đây：[https://go-acme.github.io/lego/dns/](https://go-acme.github.io/lego/dns/) |
| `DNSEnv` | không ai | Các biến môi trường bắt buộc để đăng ký chứng chỉ sử dụng DNS，Vui lòng tham khảo liên kết trên，Các thông số do nhà cung cấp dns của riêng bạn yêu cầu，điền vào đây。Lưu ý rằng một dòng，Điền vào định dạng tệp yaml。 |

