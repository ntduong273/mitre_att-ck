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

<table>
<tr>
  <th>ID</th>
  <th>Data Source</th>
  <th>Data Component</th>
  <th>Detects</th>
</tr>
<tr>
  <td>DS0022</td>
  <td>File</td>
  <td>File Creation</td>
  <td>Giám sát các hành vi tạo tệp bất thường hoặc không như mong đợi, cho thấy nội dung độc hại đã bị chèn vào thông qua các kênh giao tiếp mạng trực tuyến.<br>Phân tích 1 – Phát hiện việc tạo tệp độc hại thông qua chèn nội dung (Content Injection) <br> <code>(EventCode=11 OR source="/var/log/audit/audit.log" type="open")| where (file_type IN ("exe", "dll", "js", "vbs", "ps1", "sh", "php"))| where (process_path="C:\Users\\AppData\Local\Temp\" OR process_path="/tmp/" OR process_path="/var/tmp/")| eval risk_score=case( like(file_name, "%.exe"), 8, like(file_name, "%.js"), 9, like(file_name, "%.sh"), 7)| where risk_score >= 7| stats count by _time, host, user, file_name, process_path, risk_score</code> 
  </td>
</tr>
<tr>
  <td>DS0029</td>
  <td>Network traffic</td>
  <td>Network Traffic Content</td>
  <td>Giám sát các lưu lượng mạng bất thường cho thấy payload độc hại được truyền vào hệ thống. Sử dụng NIDS, kết hợp với kiểm tra SSL/TLS, phát hiện các payload độc hại, các dạng obfuscation nội dung, exploit code. <br> Phân tích 1 – Phát hiện chèn nội dung độc hại trong lưu lượng mạng:<br><code>(EventCode=3)OR (source="zeek_http_logs" response_code IN (302, 307) AND url IN (malicious_redirect_list))OR (source="proxy_logs" response_body_content IN (suspicious_script_list))| eval risk_score=case( response_code=302 AND url IN (malicious_redirect_list), 9, response_body_content IN (suspicious_script_list), 8, url LIKE "%@%", 7)| where risk_score >= 7| stats count by _time, host, user, url, response_code, risk_score</code>
  </td> 
</tr>
<tr>
  <td>DS0009</td>
  <td>Process</td>
  <td>Process Creation</td>
  <td>Tìm kiếm các hành vi trên endpoint, cho thấy dấu hiệu bị xâm phạm, VD: các hành vi bất thường của browser process. Bao gồm: Các tệp đáng ngờ được write ổ đĩa, bằng chứng của Process Injection (chèn tiến trình) nhằm che giấu việc thực thi.
Phân tích 1 – Phát hiện thực thi tiến trình độc hại từ nội dung bị chèn. <br> <code>(EventCode=1 OR source="/var/log/audit/audit.log" type="execve")| where (parent_process IN ("chrome.exe", "firefox.exe", "edge.exe", "safari.exe", "iexplore.exe"))| where (process_name IN ("powershell.exe", "cmd.exe", "wget", "curl", "bash", "python"))| eval risk_score=case( process_name IN ("powershell.exe", "cmd.exe"), 9, process_name IN ("wget", "curl"), 8, parent_process IN ("chrome.exe", "firefox.exe"), 7)| where risk_score >= 7| stats count by _time, host, user, process_name, parent_process, risk_score</code>
  </td>
</tr>
</table>

