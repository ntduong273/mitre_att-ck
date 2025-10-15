# Content Injection

Xâm nhập, duy trì = cách chèn payload độc hại vào system thông qua lưu lượng mạng trực tuyến. 

Thay vì dụ victims truy cập vào 1 payload độc hại trên 1 website(Drive-by Compromise) thì có thể initial access thông qua các **kênh dữ liệu trực tuyến bị xâm nhập** - nơi attacker thao túng, kiểm soát lưu lượng, chặn/chèn nội dung độc hại.

Chèn nội dung vào hệ thống nạn nhân theo nhiều cách khác nhau, bao gồm:

- Từ giữa (From the middle): khi attacker đứng giữa luồng giao tiếp hợp lệ giữa client và server trực tuyến.
- Từ bên cạnh (From the side): khi payload độc hại được chèn vào 1 fake response, gửi nhanh hơn response hợp lệ từ server thật tới client.

=> the result of compromised upstream communication channel: can thiệp vào các kênh đường truyền mà lưu lượng Internet của victims đi qua, chặn bắt, sửa đổi gói tin qua lại.

## Procedure Examples (Ví dụ thủ thuật / Quy trình thực hiện)

| ID | Name | Description |
|----|------|-------------|
| S1088 | Disco | 1 loại mã độc (custom implant) đạt được initial access và execute malware bằng content injection vào các DNS, HTTP, SMB response gửi đến các hosts mục tiêu, redirect chúng download các malware, C2 |
| G1019 | MoustachedBouncer | nhóm gián điệp mạng (cyberespionage) chuyên tấn công vào các đại sứ quán nước ngoài tại Belarus, chèn content vào các DNS, HTTP, SMB response để redirect tới một trang **Windows Update** giả nhằm tải xuống phần mềm độc hại |


## Mitigations (Biện pháp giảm thiểu / Phòng ngừa)

| ID | Mitigation | Description |
|----|------------|-------------|
| M1041 | Encrypt Sensitive Information | **Mã hóa đúng cách** lưu lượng trực tuyến thông qua các dịch vụ như **VPN đáng tin cậy**, mã hóa thông tin ở lúc nghỉ, lúc di chuyển, lúc xử lí |
| M1021 | Restrict Web-Based Content | **Chặn việc tải/chuyển và thực thi** các loại tệp ít phổ biến, đáng ngờ, đã xuất hiện trong các cuộc tấn công ở quá khứ |


## Detection

| ID | Data Source | Data Component | Detects |
|----|-------------|----------------|---------|
| DS0022 | File | File Creation | Giám sát các hành vi **tạo tệp bất thường hoặc không như mong đợi**, cho thấy nội dung độc hại đã bị chèn vào thông qua các kênh giao tiếp mạng trực tuyến.

**Phân tích 1 – Phát hiện việc tạo tệp độc hại thông qua chèn nội dung (Content Injection)**

```(EventCode=11 OR source="/var/log/audit/audit.log" type="open")| where (file_type IN ("exe", "dll", "js", "vbs", "ps1", "sh", "php"))| where (process_path="C:\Users\\AppData\Local\Temp\" OR process_path="/tmp/" OR process_path="/var/tmp/")| eval risk_score=case( like(file_name, "%.exe"), 8, like(file_name, "%.js"), 9, like(file_name, "%.sh"), 7)| where risk_score >= 7| stats count by _time, host, user, file_name, process_path, risk_score``` | 

| DS0029 | Network traffic | Network Traffic Content | 
| DS0009 | Process | Process Creation | 
