# External Remote Services

Lợi dụng các services remote access hướng ra bên ngoài để initial access, duy trì tồn tại trong mạng, dù cho có các gateway dịch vụ từ xa để quản lý kết nối và xác thực thông tin đăng nhập cho những dịch vụ này.

Sử dụng các tài khoản hợp lệ để access services, những tài khoản này có thể bị thu thập thông qua credential pharming, lấy từ người dùng sau khi xâm nhập vào mạng doanh nghiệp. 

Truy cập cũng có thể đạt được thông qua dịch vụ lộ ra ngoài mà không yêu cầu xác thực.

-----------------------------------------------------------------------------------------------------------------------------

## Procedure Example

| ID | Name	| Description |
|----|------|-------------|
| C0028	| 2015 Ukraine Electric Power Attack | During the 2015 Ukraine Electric Power Attack, Sandworm Team installed a modified Dropbear SSH client as the backdoor to target systems. <br>
| G1024	| Akira	| Akira uses compromised VPN accounts for initial access to victim networks. | 
| G0026	| APT18	| APT18 actors leverage legitimate credentials to log into external remote services.

-----------------------------------------------------------------------------------------------------------------------------

## Mitigations

| ID | Mitigation |	Description |
|----|------------|-------------|
| M1042	| Disable or Remove Feature or Program | Disable/block remotely available services unnecessary. |
| M1035	| Limit Access to Resource Over Network	| Limit access to remote services through centrally managed concentrators. |
| M1032 | Multi-factor Authentication | Using strong two-factor/multi-factor authentication for remote service accounts to mitigate an adversary's ability to leverage stolen credentials. | 
| M1030 |	Network Segmentation | Deny direct remote access to internal systems through the use of network proxies, gateways, and firewalls. |

-----------------------------------------------------------------------------------------------------------------------------

## Detection 

| ID | Data Source | Data Component | Detects |
|----|-------------|----------------|---------|
| DS0015 | Application Log | Application Log Content | When authentication không được required to access an exposed remote service thì phải monitor cho các activities kế tiếp: Sử dụng bất thường (anomalous) từ bên ngoài đối với API hoặc ứng dụng |

*Analytic 1 - Failed connection attempts from remote services*

Nguồn log: <code>index="remote_access_logs" sourcetype="vpn_logs" OR sourcetype="rdp_logs" OR sourcetype="citrix_logs"</code><br>

- Chỉ định index chứa logs liên quan đến truy cập từ xa, là nơi lưu trữ dữ liệu từ các dịch vụ như VPN, RDP, hoặc Citrix. 
- Lọc logs theo loại nguồn (sourcetype). Bao gồm:
  + vpn_logs: Logs từ VPN
  + rdp_logs: Logs từ Remote Desktop Protocol 
  + citrix_logs: Logs từ Citrix (dịch vụ remote access phổ biến cho enterprise)


<code>| stats count by src_ip, dest_ip, user, status, _time</code><br>

- Thống kê, count số lượng events cho mỗi nhóm.
  + src_ip: IP nguồn 
  + dest_ip: IP đích
  + user: Tên người dùng cố gắng kết nối
  + status: Trạng thái kết nối 
  + _time: Thời gian sự kiện

- Kết quả: Tạo một bảng đếm số lần kết nối theo các nhóm này.


<code>| where status="failed" AND count > 5</code><br>

- Lọc kết quả: chỉ các event có status="failed" và count > 5


<code>| table _time, user, src_ip, dest_ip, status</code>

- Hiển thị kết quả dưới dạng bảng với các cột: thời gian, user, IP nguồn, IP đích, status.


| ID | Data Source | Data Component | Detects |
|----|-------------|----------------|---------|
| DS0028 | Logon Session | Logon Session Metadata | Collect authentication logs and analyze for unusual access patterns, windows of activity, and access outside of normal business hours |
| DS0029 | Network Traffic | Network Connection Creation | Giám sát các newly network connections được constructed để có thể dùng Valid Accounts to access and/or persist within a network using External Remote Services. Nó có thể hợp pháp, tùy thuộc vào môi trường và cách sử dụng, vậy nên sau khi diễn ra đăng nhập, giám sát các mẫu truy cập và hoạt động có thể chỉ ra hành vi đáng ngờ, độc hại | 


*Analytic 1 - Connections to common remote service ports*

<code>index=network sourcetype="network_traffic"</code><br>

- Lấy các logs trong index network, loại log network_traffic


<code>| stats count by src_ip, dest_ip, dest_port, protocol</code><br>

<code>| where dest_port=22 OR dest_port=3389 OR dest_port=8443</code><br>

- Lọc lấy các log có dest_port là 22 (SSH), 3389 (RDP), 8443 (HTTPS).

<code>| table _time, src_ip, dest_ip, dest_port, protocol</code>


| ID | Data Source | Data Component | Detects |
|----|-------------|----------------|---------|
| DS0029 | Network Traffic | Network Traffic Content | Analyze traffic to detect anomalous requests, API usage, or data transfers. Content bất thường mà nằm trong network traffic: gọi unexpected API, file transfers, or large data uploads | 


*Analytic 1 - Large data transfers over remote service connections*

<code>index=network sourcetype="network_packet_capture"</code><br>
<code>| stats count by src_ip, dest_ip, data_size, protocol</code><br>
<code>| where data_size > 1000000</code><br>
<code>| table _time, src_ip, dest_ip, data_size, protocol</code>


| ID | Data Source | Data Component | Detects |
|----|-------------|----------------|---------|
| DS0029 | Network Traffic | Network Traffic Flow | Theo dõi cái flow of traffic to/from external sources to detect unusual patterns | 


*Analytic 1 - High-Volume data transfers*

<code>index=network sourcetype="network_traffic_flow"</code><br>
<code>| stats count by src_ip, dest_ip, bytes_sent, bytes_received</code><br>
<code>| where bytes_sent > 1000000 OR bytes_received > 1000000</code><br>
<code>| table _time, src_ip, dest_ip, bytes_sent, bytes_received</code>
