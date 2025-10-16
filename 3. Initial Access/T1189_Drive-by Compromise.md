# Drive-by Compromise 

Lừa user truy cập 1 website có chứa exploit code. Cách đưa exploit code vào:
- 1 web hợp pháp bị xâm phạm, cho phép chèn mã độc.
- file scripts cho web hợp pháp từ một bucket lưu trữ đám mây cho phép ghi công khai bị attacker sửa đổi.
- web tích hợp, cho phép content do user kiểm soát bị lợi dụng để chèn script/iframe độc hại (XSS).
- ...

Thường web được sử dụng là những trang uy tín, hay được truy cập — vd: trang thuộc chính phủ,... Chiến dịch nhắm mục tiêu kiểu này thường được gọi là strategic web compromise hoặc watering hole attack. 

Quy trình điển hình:
- Victim truy cập 1 web chứa nội dung do attacker kiểm soát.
- Scripts tự động thực thi, check version browser, plugin, dò xem có phải ver yếu hay không.
- Nếu tìm thấy, exploit code được chuyển tới browser.
- Nếu exploit success, đạt được việc execute code trên hệ thống user trừ khi có biện pháp bảo vệ khác.


## Procedure Examples

| ID | Name | Description |
|----|------|-------------|
| G1038 | Andariel | use các cuộc tấn công watering hole, kết hợp lỗ hổng zero-day, giành quyền truy cập ban đầu vào các victims nằm trong một phạm vi địa chỉ IP cụ thể. | 
| G0073 | APT19 | thực hiện cuộc tcông watering hole trên trang forbes.com vào năm 2014,  nhằm xâm nhập và chiếm quyền kiểm soát các mục tiêu. |
| G0001 | Axiom | used watering hole attacks to gain access |
| ..... | ...... |


## Mitigations

| ID | Mitigation | Description |
|----|------------|-------------|
| M1048 | Application Isolation and Sandboxing | Dùng sandbox browser giảm bớt 1 phần tác động khi bị khai thác, vẫn có thể tồn tại lỗ hổng vượt thoát sandbox. |
| M1050 | Exploit Protection | Dùng Windows Defender Exploit Guard (WDEG) và Enhanced Mitigation Experience Toolkit (EMET) để giảm hành vi khai thác or triển khai kiểm tra tính toàn vẹn luồng điều khiển (Control Flow Integrity, CFI) |
| M1021 | Restrict Web-Based Content | Adblocker có thể giúp ngăn malware phát tán thông qua các quảng cáo trước khi chúng kịp thực thi hoặc cài đặt các tiện ích mở rộng chặn script |
| M1051 | Update Software | Update browser, plugins, extensions thường xuyên | 
| M1017 | User Training | Training nhận biết các nỗ lực truy cập hoặc thao túng, làm giảm nguy cơ bị tấn công spearphishing, social engineering | 


## Detection 

<table>
  <tr>
    <th>ID</th>
    <th>Data Source</th>
    <th>Data Component</th>
    <th>Detects</th>
  </tr>
  <tr>
    <th>DS0015</th>
    <th>Application Log</th>
    <th>Application Log Content</th>
    <th>Firewall, proxy có thể check URL để detect domain hoặc arg có dấu hiệu độc hại đã biết, phân tích dựa trên uy tín (reputation-based analytics) của các trang web và tài nguyên mà user truy cập</th>
  </tr>
  <tr>
    <th>DS0022</th>
    <th>File</th>
    <th>File Creation</th>
    <th>Giám sát các hành động tạo/ghi tệp vào bộ nhớ nhằm gain access khi user truy cập web bình thường (Các process của browser tạo file ở những location đáng ngờ, check các payload: DLL, js, shellcode,...)</th>
  </tr>
  <tr>
    <th>DS0029</th>
    <th>Network Traffic</th>
    <th>Network Connection Creation & Network Traffic Content</th>
    <th>Giám sát các kết nối mạng được tạo tới các server không tin cậy dùng để gửi/nhận data (process, IP, domain, DNS query). Phát hiện các hành vi thực thi scripts đáng ngờ trên HTTPS, check JS payloads with obfuscation/encoded execution.</th>
  </tr>
  <tr>
    <th>DS0009</th>
    <th>Process</th>
    <th>Process Creation</th>
    <th>Monitor các hvi trên endpoint có dấu hiệu bị xâm phạm thành công(các hvi bất thường của browser process: ghi tệp khả nghi, process injection)</th>
  </tr>
</table>
