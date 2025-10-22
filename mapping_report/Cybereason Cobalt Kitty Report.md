# Cybereason Cobalt Kitty Report

Là một chiến dịch APT có 5 phase, nhắm vào các công ty toàn cầu.

Sử dụng một tools pentest là Cobalt Strike, được dùng cả bởi redteam và attacker - bị lạm dụng rộng rãi bởi các nhóm tội phạm mạng và APT (Advanced Persistent Threat) do tính linh hoạt và mạnh mẽ.
- Beacon Payload: cốt lõi là Beacon, agent được triển khai trên victim's sys để thiết lập kết nối với C2, cho phép thực hiện các lệnh từ xa như thu thập dữ liệu, di chuyển ngang, và thực thi mã,...
- Post-Exploitation: support các hoạt động sau xâm nhập, bao gồm đánh cắp thông tin xác thực, leo thang đặc quyền, và triển khai thêm payload.

-----------------------------------------------------------------------------------------------------------------------------------

## Phase 1: Penetration

**Tactic theo MITRE ATT&CK: TA0043 - Recon, TA0042 - rd, TA0001 - initial access, TA0002 - exe**

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 1 | TA0001 Initial Access | T1566 Phishing | .002 Spearphishing Link | Nhóm tấn công sử dụng phương pháp social engineering (kỹ nghệ xã hội), cụ thể là kiểu tấn công spear-phishing email, gửi một email giả mạo có mục đích:<br>- Link tới 1 site độc hại, download 1 fake Flash Installer có chứa Cobalt Strike Beacon. |
| 2 | TA0001 Initial Access | T1566 Phishing | .001 Spearphishing Attachment | Nhóm tấn công sử dụng phương pháp social engineering (kỹ nghệ xã hội), cụ thể là kiểu tấn công spear-phishing email, gửi một email giả mạo có mục đích:<br>- Có 1 file Word chứa các macro độc hại sẽ download các Cobalt Strike payloads. |

Giải thích cụ thể:

<img width="1402" height="568" alt="image" src="https://github.com/user-attachments/assets/f48b1726-5dec-4e14-bd5c-93b60bdbb552" />


<br><br><br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 3 | TA0002 Execution | T1204 User Execution | .001 Malicious link | Đường link độc hại dẫn tới một trang tải xuống file fake Flash installer, sau đó file fake này mới chạy các scripts, code, tạo các tiến trình,... |
| 4 | TA0002 Execution | T1059 Command and Scripting Interpreter | .003 Windows Command Shell | Đầu tiên, fake Flash installer nó sẽ gọi 1 tiến trình <code>cmd.exe</code> lên để thực hiện scripts, chạy <code>install_flashplayers.exe</code>. |
| 5 | TA0002 Execution | T1059 Command and Scripting Interpreter | .001 PowerShell | Tiến trình <code>install_flashplayers.exe</code> có 7 tiến trình con, cụ thể nó có gọi 1 tiến trình <code>powershell.exe</code> lên để thực thi code (Tiêm tiến trình). |
| 6 | TA0005 Defense Evasion | T1055 Process Injection | .001 Dynamic-link Library Injection | Sử dụng <code>powershell.exe</code> để tiến hành tiêm tiến trình <code>install_flashplayers.exe</code> vào một tiến trình hệ thống hợp pháp là <code>rundll32.exe</code>. Mục đích để che giấu hoạt động độc hại bằng cách tận dụng tiến trình hợp pháp, giúp tránh bị phát hiện bởi phần mềm diệt virus hoặc EDR|

Giải thích cụ thể:

<img width="1406" height="856" alt="image" src="https://github.com/user-attachments/assets/afd84e8f-26ab-45fb-a76e-53112bcdd1c7" />


<br><br><br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 7 | TA0011 Command and Control | T1071 Application Layer Protocol | .001 Web Protocols | Mã assembly dịch ngược của malware fake Flash installer cho thấy nó có mở một kết nối đến C2 có địa chỉ là <code>http://110.10.179.65:80/ptF2</code> để tải 1 payload với shellcode được mã hóa. Kết nối được thực hiện trên giao thức HTTP dựa vào cổng 80. |

Giải thích cụ thể:

<img width="1247" height="829" alt="image" src="https://github.com/user-attachments/assets/21c46ebc-16b5-448a-9fce-e00d00c6fff4" />

<br><br><br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 8 | TA0002 Execution | T1204 User Execution | .002 Malicious File | Một kiểu tấn công spear-phishing email khác có đính kèm 1 file .doc có chứa các macro độc hại, được đặt tên dựa theo các tài liệu hợp pháp, nhằm đánh lừa nạn nhân mở file .doc này. |
| 9 | TA0002 Execution | T1053 Scheduled Task/Job | .005 Scheduled Task | Khi mở file .doc kia, các macro độc hại sẽ được thực thi, tạo ra 2 cái task được lên lịch sẵn, thực hiện hành động tải xuống các file được ngụy trang dưới dạng đuôi .jpg từ C2. |

Dẫn chứng:

<img width="1408" height="478" alt="image" src="https://github.com/user-attachments/assets/2fc42ec4-a08b-4574-b704-cbd3453c6693" />

<br><br><br>

**Sau khi lây nhiễm, tiến hành thực thi các scheduled task**


| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 10 | TA0005 Defense Evasion | T1218 System Binary Proxy Execution | .005 Mshta | Lợi dụng mshta.exe là một công cụ Windows uy tín làm proxy để execute các malicious .hta files, js, VBScript |

Dẫn chứng:

<img width="1400" height="590" alt="image" src="https://github.com/user-attachments/assets/89cba533-4e5b-4ca2-b754-479559d1c07d" />

<br><br><br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 11 | TA0011 Command & Control | T1105 Ingress Tool Transfer | No sub-technique | Kẻ tấn công vận chuyển các tool, file độc hại khác từ C2 vào compromised environment. Các tool, file này có thể đc copy từ 1 system bên ngoài do attacker kiểm soát tới network's victim qua kênh C2/protocol vận chuyển khác as ftp. Sau đó, attacker phân tán các tools giữa các thiết bị of victim nhằm mở rộng kiểm soát, lateral movement|

Dẫn chứng:

<img width="1406" height="643" alt="image" src="https://github.com/user-attachments/assets/eb5f867f-fcb4-4210-86d3-2cc73ce705b0" />

<br><br><br>

Ngoài việc dùng lại kĩ thuật lợi dụng powershell.exe để thực thi malicious code thì ở bước này còn sử dụng các kĩ thuật che dấu payload độc hại bằng các phương pháp làm rối (obfuscated, XOR), nhúng mã vào file ảnh/giả dạng file ảnh có đuôi .jpg (steganography) để tránh bị defense của victim phát hiện.

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 12 | TA0005 Defense Evasion | T1027 Obfuscated Files or Information | .010 Command Obfuscation | Làm rối các lệnh/script bằng cách mã hóa hoặc thay đổi cú pháp, thứ tự các kí tự,... khiến cho câu lệnh/script khó có thể đọc được bằng mắt thường hay tránh phát hiện bởi các IDS,AV do không so khớp được mẫu. |
| 13 | TA0005 Defense Evasion | T1027 Obfuscated Files or Information | .003 Steganography | Nhúng mã độc trong file hợp pháp (image.jpg) để ngụy trang, thực chất là payload mã hóa, sử dụng steganography để trông như file hình ảnh vô hại. |

Dẫn chứng:

<img width="983" height="838" alt="image" src="https://github.com/user-attachments/assets/d07457c6-d06a-448f-9c08-e97dc24f2f28" />

-----------------------------------------------------------------------------------------------------------------------------------

## Phase 2: Foothold and persistence

Sử dụng 3 kĩ thuật như sau:

<img width="1402" height="369" alt="image" src="https://github.com/user-attachments/assets/36af095f-d4db-4838-8742-898ef280f695" />

<br><br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 14 | TA0003 Persistence | T1547 Boot or Logon Autostart Execution | .001 Registry Run Keys / Startup Folder | Attacker sử dụng Windows Registry Autorun để execute VBScript và PowerShell scripts nhằm trú ngụ trong thư mục ProgramData, nơi mà sẽ được ẩn đi theo mặc định |
| 15 | TA0003 Persistence | T1543 Create or Modify System Process | .003 Windows Service | Attacker created, modified các Windows Services để đảm bảo việc loading các PowerShell scripts on the compromised machines, hầu hết là Cobalt Strike’s Beacon payloads đã được mã hóa bằng Powershell |
| 16 | TA0003 Persistence | T1053 Scheduled Task/Job | .005 Scheduled Task | Đảm bảo rằng payloads độc sẽ được thực thi theo khung thời gian xác định trước |

<br><br>

Ở trong kỹ thuật sử dụng Windows Registry, attacker còn sử dụng thêm kĩ thuật T1096 NTFS File Attributes, là một trick khá cũ để ẩn đi các payloads độc hại, tránh bị phát hiện.

<img width="1086" height="801" alt="image" src="https://github.com/user-attachments/assets/b2dbf7a4-134e-4bad-b299-4d53b78e22b5" />

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 17 | TA0005 Defense Evasion | T1564 Hide Artifacts | .004 NTFS File Attributes | Mỗi NTFS formatted partition đều chứa 1 Master File Table (MFT) để duy trì bản ghi cho mọi tệp/thư mục trên phân vùng. Trong các mục nhập của MFT, có các thuộc tính tệp, có thể được sử dụng để lưu trữ dữ liệu tùy ý => giấu payload độc hại | 

<br><br>

<img width="1090" height="812" alt="image" src="https://github.com/user-attachments/assets/5ca35d66-c8c7-414a-b22f-a694a9d87249" />

Ngoài ra, còn có một phương án persistence khác là sử dụng backdoor khai thác lỗ hổng DLL Hijacking đối với Wsearch. Lợi dụng Wsearch là một thành phần mặc định của Windows, tự động chạy. Khi Wsearch khởi động, nó sẽ kích hoạt các ứng dụng SearchIndexer.exe và SearchProtocolHost.exe - tồn tại điểm yếu dễ bị tấn công. Từ đó attacker đặt một tệp “msfte.dll” giả mạo trong thư mục system32 làm cho tệp “msfte.dll” giả mạo sẽ được nạp mỗi khi Wsearch khởi chạy các ứng dụng này.

Kỹ thuật này có thể được map sang MITRE ATT&CK theo định danh:

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 18 | TA0003 Persistence | T1574 Hijack Execution Flow | .001 DLL | DLL Search Order Hijacking: search order - thứ tự tìm kiếm mà Windows sử dụng để nạp DLL, attacker có thể đặt một DLL Trojan vào một thư mục sẽ được ưu tiên bởi thứ tự tìm kiếm DLL, khiến Windows nạp DLL độc hại khi chương trình nạn nhân gọi đến nó. |

Hoặc sử dụng các macro của Outlook, sửa các registry để khởi động mỗi khi boot:

<img width="1128" height="657" alt="image" src="https://github.com/user-attachments/assets/6dd9ab41-020b-4fa1-b3b4-b4c293fe9d41" />

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 19 | TA0003 Persistence | T1137 Office Application Startup | .001 Office Template Macros | Sử dụng một macro độc hại dạng backdoor trên Outlook để giao tiếp và trích xuất dữ liệu ra ngoài C2, đồng thời sửa 1 giá trị registry đặc biệt để duy trì truy cập |

-----------------------------------------------------------------------------------------------------------------------------------

## Phase 3: Command & control and data exfiltration




-----------------------------------------------------------------------------------------------------------------------------------

## Phase 4: Internal reconnaissance


## Phase 5: Lateral movement

