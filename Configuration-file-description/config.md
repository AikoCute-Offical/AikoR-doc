# Mô tả tệp cấu hình

## Định dạng tệp cấu hình

1. Tệp cấu hình chính có `yaml` Định dạng，được đặt tên `xxx.yml` 。
2. Theo mặc định, AikoR sẽ sử dụng các tệp trong thư mục chạy phần mềm.`aiko.yml` dưới dạng tệp cấu hình.

Định dạng cơ bản của tệp cấu hình, nhiều bảng và nhiều thông tin cấu hình nút có thể được thêm vào cùng một lúc trong Nodes, chỉ cần thêm các mục Nodes theo cùng một định dạng.

```yaml
Log:
  Level: none # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/AikoR/access.Log
  ErrorPath: # /etc/AikoR/error.log
DnsConfigPath: # /etc/AikoR/dns.json # Path to dns config, check https://xtls.github.io/config/dns.html for help
RouteConfigPath: # /etc/AikoR/route.json # Path to route config, check https://xtls.github.io/config/routing.html for help
InboundConfigPath: # /etc/AikoR/custom_inbound.json # Path to custom inbound config, check https://xtls.github.io/config/inbound.html for help
OutboundConfigPath: # /etc/AikoR/custom_outbound.json # Path to custom outbound config, check https://xtls.github.io/config/outbound.html for help
ConnetionConfig:
  Handshake: 4 # Handshake time limit, Second
  ConnIdle: 10 # Connection idle time limit, Second
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second
  BufferSize: 64 # The internal cache size of each connection, kB 
Nodes:
  -
    PanelType: "SSpanel" # Panel type: SSpanel, V2board, PMpanel, Proxypanel
    ApiConfig:
      ApiHost: "http://127.0.0.1:667"
      ApiKey: "123"
      NodeID: 41
      NodeType: V2ray # Node type: V2ray, Trojan, Shadowsocks, Shadowsocks-Plugin
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/AikoR/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      DisableUploadTraffic: false # Disable Upload Traffic to the panel
      DisableGetRule: false # Disable Get Rule from the panel
      DisableIVCheck: false # Disable the anti-reply protection for Shadowsocks
      DisableSniffing: false # Disable domain sniffing 
      EnableProxyProtocol: false
      AutoSpeedLimitConfig:
        Limit: 0 # Warned speed. Set to 0 to disable AutoSpeedLimit (mbps)
        WarnTimes: 0 # After (WarnTimes) consecutive warnings, the user will be limited. Set to 0 to punish overspeed user immediately.
        LimitSpeed: 0 # The speedlimit of a limited user (unit: mbps)
        LimitDuration: 0 # How many minutes will the limiting last (unit: minute)
      RedisConfig:
        RedisLimit: 0 # The Redis limit of a user, 0 means disable
        RedisAddr: 127.0.0.1:6379 # The redis server address format: (IP:Port)
        RedisPassword: PASSWORD # Redis password
        RedisDB: 0 # Redis DB (Redis database number, default 0, no need to change)
        Timeout: 5 # Timeout for Redis request
        Expiry: 60 # Expiry time ( Cache time of online IP, unit: second )
      EnableFallback: false # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Alpn: # Alpn, Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        RejectUnknownSni: false # Reject unknown SNI
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/AikoR/cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: /etc/AikoR/cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board
    ApiConfig:
      ApiHost: "http://V2board.com"
      ApiKey: "123"
      NodeID: 42
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
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

## Hướng dẫn Cài đặt Cấu hình

### Cấu hình cơ bản

Cấu hình cơ sở là cấu hình có hiệu lực trên tất cả các nút.
```yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/AikoR/access.Log
  ErrorPath: # /etc/AikoR/error.log
DnsConfigPath: # /etc/AikoR/dns.json # Path to dns config, check https://xtls.github.io/config/dns.html for help
RouteConfigPath: # /etc/AikoR/route.json # Path to route config, check https://xtls.github.io/config/routing.html for help
InboundConfigPath: # /etc/AikoR/custom_inbound.json # Path to custom inbound config, check https://xtls.github.io/config/inbound.html for help
OutboundConfigPath: # /etc/AikoR/custom_outbound.json # Path to custom outbound config, check https://xtls.github.io/config/outbound.html for help
ConnetionConfig:
  Handshake: 4 # Handshake time limit, Second
  ConnIdle: 10 # Connection idle time limit, Second
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second
  BufferSize: 64 # The internal cache size of each connection, kB
```

#### cấu hình nhật ký

Cấu hình nhật ký được sử dụng để kiểm soát cấp độ nhật ký của AikoR-core，access.log and error.log, Bạn cần đặt cấp độ nhật ký lớn hơn cảnh báo để được ghi nhật ký。

```yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/AikoR/access.Log
  ErrorPath: # /etc/AikoR/error.log
```

| tham số      | Tùy chọn                                | Minh họa                                   |
| ------------ | --------------------------------------- | -------------------------------------------|
| `Level`      | `none`,`error`,`warning`,`info`,`debug` | mức hiển thị nhật ký，`none`không hiển thị |
| `AccessPath` | không ai                                | Access đường dẫn lưu nhật ký               |
| `ErrorPath`  | không ai                                | Error đường dẫn lưu nhật ký                |

#### Cấu hình DNS tùy chỉnh

Chỉ định đường dẫn đến tệp cấu hình DNS tùy chỉnh

```yaml
DnsConfigPath: # /etc/AikoR/dns.json  Path to dns config
```

| tham số            | Tùy chọn | Minh họa                   |
| --------------- | ---- | ----------------------- |
| `DnsConfigPath` | không ai  | Đường dẫn đến tệp cấu hình DNS tùy chỉnh |
#### Cấu hình định tuyến tùy chỉnh

Chỉ định đường dẫn tệp cấu hình tuyến đường

```yaml
RouteConfigPath: # /etc/AikoR/route.json # Path to route config, check https://xtls.github.io/config/base/route/ for help
```

| tham số              | Tùy chọn | Minh họa                    |
| ----------------- | ---- | ------------------------ |
| `RouteConfigPath` | không ai   | Đường dẫn đến tệp cấu hình định tuyến tùy chỉnh |

#### Cài đặt mục nhập tùy chỉnh

```yaml
InboundConfigPath: # /etc/AikoR/custom_inbound.json # Path to custom inbound config, check https://xtls.github.io/config/inbound.html for help
```

| tham số                | Tùy chọn | Minh họa                     |
| ------------------- | ---- | ------------------------ |
| `InboundConfigPath` | không ai   | Đường dẫn đến tệp cấu hình mục nhập tùy chỉnh |
#### Cấu hình xuất tùy chỉnh
Chỉ định đường dẫn tệp hồ sơ xuất
```yaml
OutboundConfigPath: # /etc/AikoR/custom_outbound.json # Path to custom outbound config, check https://xtls.github.io/config/base/outbound/ for help
```

| tham số                 | Tùy chọn | Minh họa                     |
| -------------------- | ---- | ------------------------ |
| `OutboundConfigPath` | không ai   | Đường dẫn đến tệp cấu hình xuất tùy chỉnh |

#### kiểm soát kết nối

Cấu hình liên quan của bản phát hành kết nối tùy chỉnh，Có thể tối ưu hóa việc sử dụng bộ nhớ ở một mức độ nhất định

```yaml
ConnetionConfig:
  Handshake: 4 # Handshake time limit, Second
  ConnIdle: 10 # Connection idle time limit, Second
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second
  BufferSize: 64 # The internal cache size of each connection, kB
```

| tham số         | Tùy chọn| Minh họa                            |
| -------------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Handshake`    | không ai   | Giới hạn thời gian bắt tay khi kết nối được thiết lập. Đơn vị là giây. Giá trị mặc định là 4. Khi proxy gửi đến xử lý một kết nối mới, nếu nó mất nhiều thời gian hơn thời gian này trong giai đoạn bắt tay, kết nối đó sẽ bị hủy bỏ.                                                              |
| `ConnIdle`     | không ai   | Giới hạn thời gian để kết nối không hoạt động. Đơn vị là giây. Giá trị mặc định là 10. nếu trong `ConnIdle` Trong thời gian này, không có dữ liệu nào được truyền (bao gồm cả dữ liệu đường lên và đường xuống), kết nối bị chấm dứt.**Giảm giá trị này có thể tối ưu hóa việc sử dụng bộ nhớ, nhưng nó sẽ dẫn đến độ trễ kết nối người dùng cao hơn**。 |
| `UplinkOnly`   | không ai   | Giới hạn thời gian mà đường xuống của kết nối bị đóng. Đơn vị là giây. Giá trị mặc định là 2. Khi máy chủ (chẳng hạn như một trang web từ xa) đóng kết nối đường xuống, proxy gửi đi đang chờ`UplinkOnly`Ngắt kết nối sau thời gian.                                                      |
| `DownlinkOnly` | không ai   | Giới hạn thời gian mà sau đó đường lên được kết nối sẽ bị đóng. Đơn vị là giây. Giá trị mặc định là 4. Khi máy chủ (chẳng hạn như một trang web từ xa) đóng kết nối ngược dòng, proxy gửi đi đang chờ`DownlinkOnly`Ngắt kết nối sau thời gian.                                                    |
| `BufferSize`   | không ai   | Kích thước bộ nhớ đệm nội bộ trên mỗi kết nối。Đơn vị là kB。khi giá trị là 0 Thời gian，Bộ nhớ đệm nội bộ bị tắt。**Giảm giá trị này có thể tối ưu hóa việc sử dụng bộ nhớ, nhưng có thể dẫn đến tăng mức sử dụng CPU**                                                                   |
dấu： 1. giảm `ConnIdle` Có thể tối ưu hóa việc sử dụng bộ nhớ khi số lượng kết nối nhiều, nhưng nó sẽ dẫn đến độ trễ kết nối của người dùng cao hơn. 2. Trong trường hợp duyệt HTTP, bạn có thể sử dụng `UplinkOnly` và `DownlinkOnly` đặt thành 0，Để cải thiện hiệu quả của việc đóng kết nối，Giảm mức sử dụng bộ nhớ. 3. giảm `BufferSize` Việc sử dụng bộ nhớ có thể được tối ưu hóa, nhưng có thể khiến mức sử dụng CPU tăng lên.

### Cấu hình nút

Mỗi nút là một cấu hình độc lập và sẽ không ảnh hưởng đến nhau. AikoR hỗ trợ khởi động và kết nối đa nút đơn phiên bản với nhiều nút cùng một lúc.

```yaml
Nodes:
  -
    PanelType: "SSpanel" # Panel type: SSpanel, V2board, PMpanel
    ApiConfig:
      ApiHost: "http://127.0.0.1:667"
      ApiKey: "123"
      NodeID: 41
      NodeType: V2ray # Node type: V2ray, Trojan, Shadowsocks, Shadowsocks-Plugin
      Timeout: 30 # Timeout for the api request, Default is 5 sec
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/AikoR/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      DisableUploadTraffic: false # Disable Upload Traffic to the panel
      DisableGetRule: false # Disable Get Rule from the panel 
      EnableProxyProtocol: false # Only works for WebSocket and TCP
      EnableFallback: false # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/AikoR/cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: /etc/AikoR/cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board, PMpanel
    ApiConfig:
      ApiHost: "http://V2board.com"
      ApiKey: "123"
      NodeID: 42
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/AikoR/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
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

#### lựa chọn bảng điều khiển

```yaml
PanelType: "V2board" # Panel type: SSpanel, V2board, PMpanel, Proxypanel
```

| tham số     | Tùy chọn                                                 | Minh họa             |
| ----------- | -------------------------------------------------------- | -------------------- |
| `PanelType` | `SSPanel`,`V2board`,`PMpanel`,`Proxypanel`, `V2RaySocks` | Loại bảng điều khiển phía trước gắn đế|

#### Cấu hình bảng điều khiển

```yaml
ApiConfig:
    ApiHost: "http://127.0.0.1:667"
    ApiKey: "123"
    NodeID: 41
    NodeType: V2ray # Node type: V2ray, Trojan, Shadowsocks, Shadowsocks-Plugin
    Timeout: 30 # Timeout for the api request, Default is 5 sec
    EnableVless: false # Enable Vless for V2ray Type
    EnableXTLS: false # Enable XTLS for V2ray and Trojan
    SpeedLimit: 0 # Local settings will replace remote settings, 0 means disable
    DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
    RuleListPath: # /etc/AikoR/rulelist Path to local rulelist file
    DisableCustomConfig: false # Disable custom config
```

| tham số        | Tùy chọn                         |Minh họa                               |
|----------------|----------------------------------|---------------------------------------|
| `ApiHost`      | không ai             | Gắn địa chỉ của bảng điều khiển phía trước        |
| `ApiKey`       | không ai             | Phím giao tiếp docking front-end                  |
| `NodeID`       | không ai             | nút ID                    |
| `NodeType`     | `V2ray`,`Shadowsocks`, `Shadowsocks-Plugin`,`Trojan` | Loại nút          |
| `Timeout`      | không ai             | Đặt thời gian chờ cho một lần truy cập vào API, mặc định là 5 giây    |
| `EnableVless`  | `true`,`false`       | Có bật giao thức Vless cho V2ray hay không        |
| `EnableXTLS`   | `true`,`false`       | Có sử dụng XTLS không                             |
| `SpeedLimit`   | float                | Đơn vị là Mbps, cài đặt giới hạn tốc độ cục bộ sẽ ghi đè cài đặt từ xa, 0 có nghĩa là không được bật |
| `DeviceLimit`  | int                  | Các hạn chế thiết bị cục bộ, sẽ ghi đè cài đặt từ xa, 0 không được bật  |
| `RuleListPath` | không ai             | Cài đặt quy tắc cục bộ, chỉ định đường dẫn tệp quy tắc cục bộ, định dạng tệp quy tắc  |
| `DisableCustomConfig` | `true`,`false`| Có bật không custom_config，mặc định false        |

#### Cấu hình liên quan đến phụ trợ

```yaml
ControllerConfig:
  ListenIP: 0.0.0.0 # IP address you want to listen
  SendIP: 0.0.0.0 # IP address you want to send pacakage
  UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
  EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
  DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
  DisableUploadTraffic: false # Disable Upload Traffic to the panel
  DisableGetRule: false # Disable Get Rule from the panel
  DisableIVCheck: false # Disable the anti-reply protection for Shadowsocks
  DisableSniffing: false # Disable domain sniffing 
  EnableProxyProtocol: false
  AutoSpeedLimitConfig:
    Limit: 0 # Warned speed. Set to 0 to disable AutoSpeedLimit (mbps)
    WarnTimes: 0 # After (WarnTimes) consecutive warnings, the user will be limited. Set to 0 to punish overspeed user immediately.
    LimitSpeed: 0 # The speedlimit of a limited user (unit: mbps)
    LimitDuration: 0 # How many minutes will the limiting last (unit: minute)
  EnableFallback: false # Only support for Trojan and Vless
  FallBackConfigs:  # Support multiple fallbacks
    -
      SNI: # TLS SNI(Server Name Indication), Empty for any
      Path: # HTTP PATH, Empty for any
      Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
      ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
```

| tham số                | Tùy chọn                           | Minh họa                  |
|----------------------  |------------------------------------|------------------------------------|
| `ListenIP`             | không ai                           | Chọn địa chỉ IP đang nghe，`0.0.0.0` Sẽ nghe cả v6 và v4     |
| `SendIP`               | không ai                           | Địa chỉ IP để gửi dữ liệu         |
| `UpdatePeriodic`       | không ai                           | Khoảng thời gian để cập nhật nút, thông tin người dùng và báo cáo thông tin sử dụng của người dùng từ giao diện người dùng, mặc định là 60 giây       |
| `EnableDNS`            | `true`,`false`                     | Có bật DNS tùy chỉnh cho nút hiện tại hay không, hãy sử dụng DNS hệ thống theo mặc định             |
| `DNSType`              | `AsIs`,`UseIP`,`UseIPv4`,`UseIPv6` | Loại phân giải DNS，`AsIs`：Sử dụng DNS hệ thống，`UseIP`,`UseIPv4`,`UseIPv6` Để sử dụng DNS tùy chỉnh，hãy chắc chắn rằng `EnableDNS` vì `true`，và được định cấu hình chính xác `DnsConfigPath` |
| `DisableUploadTraffic` | `false`, `true`                    | Có cấm tải lên lưu lượng nút hay không, mặc định `false`                       |
| `DisableGetRule`       | `false`, `true`                    | Có cấm lấy các quy tắc từ xa hay không, mặc định `false`                 |
| `DisableIVCheck`       | `false`, `true`                    |Có tắt bộ lọc Bloom được Shadowsocks sử dụng để ngăn các cuộc tấn công phát lại hay không, theo mặc định `false`                    |
| `DisableSniffing`      | `false`, `true`                    | Có tắt tính năng dò tìm miền hay không, mặc định `false`                |
| `EnableProxyProtocol`  | `true`,`false`                     | Có bật ProxyProtocol cho nút hiện tại để lấy IP chuyển tiếp hay không                   |
| `AutoSpeedLimitConfig` | list                               | Cấu hình liên quan đến giới hạn tốc độ động, vui lòng kiểm tra [Giới hạn tốc độ động](../Function-Description/speedlimit.md)                   |
| `EnableFallback`       | `true`,`false`                     | Có bật cho nút hiện tại hay không Fallback，Chỉ hợp lệ cho các giao thức Vless và Trojan                      |
| `FallBackConfigs`      | list                               | Fallback Cấu hình liên quan，hãy kiểm tra [Fallback Mô tả chức năng](../Function-Description/fallback.md)        |

#### Cấu hình liên quan đến ứng dụng chứng chỉ

AikoR hỗ trợ nhiều cấu hình ứng dụng chứng chỉ tự động。Chứng chỉ đã áp dụng sẽ được đặt trong**tập tin cấu hình(aiko.yml)danh mục `cert` dưới thư mục**。

```yaml
CertConfig:
    CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
    RejectUnknownSni: false # Reject unknown SNI, default false
    CertDomain: "node2.test.com" # Domain to cert
    CertFile: /etc/AikoR/cert/node2.test.com.cert # Provided if the CertMode is file
    KeyFile: /etc/AikoR/cert/node2.test.com.key
    Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
    Email: test@me.com
    DNSEnv: # DNS ENV option used by DNS provider
        ALICLOUD_ACCESS_KEY: aaa
        ALICLOUD_SECRET_KEY: bbb
```

| tham số            | Tùy chọn                         | Minh họa           |
| ------------------ | ---------------------------------| --------------------------------------------- |
| `CertMode`         | `none`,`file`,`http`,`dns`       | Cách lấy chứng chỉ。`file`:Cung cấp thủ công，và tạo một con đường。`http`：Đăng ký qua http，yêu cầu cổng 80。`dns`：Áp dụng bằng chế độ dns，Cấu hình nhà cung cấp dịch vụ dns có liên quan cần được xây dựng。`none`：buộc đóng cài đặt tls，Giao nó cho nginx hoặc caddy。 |
| `CertDomain`       | không ai                         | Đăng ký tên miền chứng chỉ             |
| `RejectUnknownSni` | `false`, `true`                  |Có từ chối SNI không xác định hay không，Mặc định là false     |
| `CertFile`         | không ai                         | Đường dẫn chứng chỉ được chỉ định theo cách thủ công                 |
| `KeyFile`          | không ai                         | Đường dẫn khóa cá nhân được chỉ định theo cách thủ công          |
| `Provider`         | không ai                         | nhà cung cấp dns，Tất cả các nhà cung cấp dns được hỗ trợ có thể được tìm thấy tại đây：[https://go-acme.github.io/lego/dns/](https://go-acme.github.io/lego/dns/)                                                   |
| `DNSEnv`           | không ai                         | Các biến môi trường bắt buộc để đăng ký chứng chỉ sử dụng DNS，Vui lòng tham khảo liên kết trên，Các thông số do nhà cung cấp dns của riêng bạn yêu cầu，Điền vào đây. Lưu ý rằng một dòng，Điền vào định dạng tệp yaml.         |
