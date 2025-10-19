# MITRE ATT&CK

Một chút ghi chép khi tìm hiểu về MITRE ATT&amp;CK của mình với vai trò là Blue Team.

ATT&CK = Adversarial Tactics Techniques & Common Knowledge (Các chiến thuật, kỹ thuật phá hoại và hiểu biết chung).

____________________________________________________________________________________________________________________________

Sưu tầm:
[PODCAST](https://www.youtube.com/watch?v=62y0unJohwM)

**Cách MITRE ATT&CK framework được dùng để phân tích các cuộc tấn công mạng thực tế trong SOC**

ATT&CK tạo ra một cái ***ngôn ngữ chung*** rất giá trị, thay vì mô tả kiểu chung chung, nó cho phép mình gọi tên chính xác từng hành vi của cái tấn công bằng các ***tactic*** - mục tiêu lớn và ***technique*** - cách làm cụ thể. Giúp các đội SOC hiểu nhau và xây phòng thủ dựa trên cái mà hacker thực sự làm.

Ví dụ một kịch bản rất quen thuộc: *Ai đó mở file, kích hoạt macro ẩn, rồi bùm, dữ liệu bị mã hóa, đòi tiền chuộc*

=> Map từng giai đoạn một của cuộc tấn công này theo khung ATT&CK, cụ thể:

<img width="2552" height="1782" alt="image" src="https://github.com/user-attachments/assets/a9f778c9-3f88-47e6-b9fe-ae4989c8f256" />

Ví dụ 2: *SIEM SOC nhận alert từ EDR, thấy 1 process powershell.exe/rundll32.exe đang cố đọc bộ nhớ của process lsass.exe.
Tiến trình lsass (Local Security Authority Subsystem Service) rất nhạy cảm, nó quản lý thông tin xác thực trên Windows sau khi user đăng nhập, hash mật khẩu các thứ,...*

<img width="2568" height="1722" alt="image" src="https://github.com/user-attachments/assets/b83a275f-4c03-42b7-aaff-21b9d6d897c9" />

**Tóm lại:** thay vì chỉ chạy theo các dấu hiệu tĩnh (IOCs, IP,...) dễ thay đổi thì ATT&CK hướng SOC vào việc hiểu và phát hiện cái hành vi, cái TTP (Tactics, Techniques, Procedures) của attacker, giúp phát hiện các threat mới hoặc biến thể mới, ngay cả khi malware thay đổi nhưng cách tấn công thì vẫn thế.

Ngoài ra thì nó còn giúp SOC analysist nhìn ra các khoảng trống, các điểm yếu trong hệ thống giám sát (Gap Analysis) giúp thiết kế lại các quy tắc giám sát hay các hoạt động Threat Hunting hiệu quả và đúng mục tiêu hơn.


____________________________________________________________________________________________________________________________

**ATT&CK Matrix for Enterprise gồm 14 tactics, mỗi tactic có techniques, sub-techniques**

## TA0043 Reconnaissance

Thăm dò, dò quét system của victim -> thu thập in4 hữu ích cho quá trình tấn công sau này.
=> Phương pháp chủ động(Scanning) & phương pháp bị động(OSINT).


## TA0042 Resource Development

Thiết lập, phát triển tài nguyên phục vụ quá trình tấn công.<br>Resource có thể là hạ tầng, account, email, service, malware, skill,... bất kì thứ gì cần thiết cho một cuộc tấn công.


## TA0001 Initial Access

Làm mọi thứ để có được một chỗ đứng(foothold) ban đầu trong victim's system.<br>Có 2 kỹ thuật chính:<br>- Kỹ thuật spear-phishing có mục tiêu<br>- Khai thác các lỗ hổng trên public website

Foothold có thể được duy trì lâu dài hoặc tồn tại tạm thời(khi hệ thống thay đổi).


## TA0002 Execution

Trying to run malicious code, malware do attacker control, có thể theo cách local hoặc remote. <br>Kết hợp với các techniques khác để đạt được nhiều mục tiêu.


## TA0003 Persistence

Trying to maintain chỗ đứng trong victim's system, ngay cả khi system restarts, thay đổi credential in4, gián đoạn kết nối, sự cố,... <br>Any access, action, configuration changes: sửa registry, adding startup code,...


## TA0004 Privilege Escalation

Trying to gain higher-level permissions on system/network bằng cách tận dụng system weakness, misconfigurations, vulnerabilities,... 
<br>Mục tiêu là các roles có đặc quyền cao: 
- SYSTEM/root
- Local admin
- ...
  

## TA0005 Defense Evasion

Trying to avoid being detected throughout quá trình compromise.

2 cách làm chính:
- Uninstall/disable security softwares
- Obfuscating/encrypting data, scripts

Hoặc lợi dụng/lạm dụng các trusted process để hide/disguise malware.


## TA0006 Credential Access

Trying to steal credential in4 (account, id, password, hash, token) by keylogging or credential dumping (lấy từ nhiều nguồn: memory, dump file hệ thống, registry,..).

Nếu chiếm được, access vào system luôn bằng legitimate credentials nhằm:
- Khó bị system phát hiện
- Tạo thêm nhiều account hợp pháp để khai thác, sử dụng.


## TA0007 Discovery

Trying to figure out (tìm hiểu) cái victim's environments, gain knowledge about system, internal networks.

Từ việc quan sát được env -> định hướng khai thác, biết phần nào có giá trị, phần nào dễ attack, phần nào dễ control,... -> có lợi cho cuộc tấn công.<br>Tận dụng luôn các tools hệ thống trên OS của victim để gather in4.


## TA0008 Lateral Movement

Trying to move through victim env, enter, control remote sys on a network.<br>-> explore ra network, find những target rồi access, exploit it.

Tuy nhiên phải xoay vòng qua nhiều system, account khác nhau để nắm bắt, bao phủ toàn bộ hệ thống. Cài thêm remote access tools để sử dụng sau này.


## TA0009 Collection

Trying to gather data useful, valueable or attacker's goal, dựa vào các source in4 có liên quan.<br>Sau khi collect xong, có thể steal (exfiltrate) or sử dụng chính các in4 đó để khai thác sâu thêm vào sys.

Các nguồn in4 hữu ích: drive types, browsers, audio, video, email, image, documents,...


## TA0011 Command & Control

Trying to communicate with compromised system to control, exploit within a victim network.<br>-> Cố gắng bắt chước các lưu lượng mạng bình thường, hợp lệ nhằm tránh bị phát hiện.

Tùy vào cấu trúc mạng, hệ thống phòng thủ của victim mà attacker xây dựng các biện pháp kết nối.

 
## TA0010 Exfiltration

Trying to steal data from victim's network. Từ bước collect, in4 được packed to avoid detection bằng cách nén(compressed) hoặc mã hóa(encrypt) rồi vận chuyển thông qua kênh C2.


## TA0040 Impact

Trying to manipulate, interrupt, destroy victim's system/data, giả mạo data, phá vỡ tính sẵn có và toàn vẹn của dữ liệu.
