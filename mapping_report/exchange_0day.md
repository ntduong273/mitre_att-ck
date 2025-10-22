# THỰC HÀNH MAPPING CÁC KĨ THUẬT, CHIẾN THUẬT TRONG CÁC CUỘC TẤN CÔNG ZERO-DAY VÀO MICROSOFT EXCHANGE


**Instructions:**

- Thời gian: Tháng 8/2022
- Hệ thống ứng dụng chứa lỗ hổng/bị khai thác: Microsoft Exchange Server
- Phân loại lỗ hổng bị khai thác: Zero-day

**Phân tích lỗ hổng:**

*1. Post auth SSRF:* khai thác lỗ hổng SSRF bằng một tài khoản đã được xác thực.


*2. RCE via powershell backend endpoint:* có tài khoản xác thực, thực hiện RCE lên server bằng các request.


**Xác định các hành vi sau khi khai thác lỗ hổng:**

Sau khi khác thác thành công 2 lỗ hổng trên, attacker thực hiện qua các hành vi sau:

- Thu thập thông tin 
- Tạo chỗ đứng trong hệ thống
- Tạo backdoor trên hệ thống
- Lateral movement

## webshell

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 1 | TA0003 Persistence | T1505 Server Software Components | .003 Webshell | Các webshell được drop xuống Exchange server, mặc dù hầu hết đã bị obfuscated nhưng mục đích vẫn là duy trì quyền truy cập, kiểm soát từ xa |
| 2 | TA0005 Defense Evasion | T1027 Obfuscated Files or Information | .010 Command Obfuscation | Các webshell bị obfuscated mục đích làm rối mã, tránh bị detect bởi các tool defense,... |

<img width="1599" height="485" alt="image" src="https://github.com/user-attachments/assets/29a669a5-4ba3-428a-b70f-e5682be85b6f" />

<br><br><br>

## command execution

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 3 | TA0003 Execution | T1059 Command and Scripting Interpreter | .003 Windows Command Shell | Lợi dụng cmd để thực hiện tải file độc hại, kiểm tra kết nối thông qua certutil có sẵn trên Windows |
| 4 | TA0003 Execution | T1047 Windows Management Instrumentation | No-subtech | Thực thi các file suspicious |

<img width="1778" height="704" alt="image" src="https://github.com/user-attachments/assets/ca293f3c-fc65-495e-9773-5a354fcf7bb4" />
<br><br><br>

## suspicious file

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 5 | TA0006 Credential Access | T1003 OS Credential Dumping | .001 LSASS Memory | Các file all.exe và dump.dll trích xuất thông tin tài khoản trên hệ thống, sử dụng rar.exe để nén các file dump và copy ra webroot của máy chủ Exchange |
| 6 | TTA0009 Collection | T1560 Archive Collected Data | .001 Archive via Utility | Sử dụng WinRAR rar.exe để nén các file dump sau quá trình thu thập thông tin xác thực ở trên |

<img width="1532" height="732" alt="image" src="https://github.com/user-attachments/assets/a3a942dd-8d9f-4c66-8cbb-248fddac2ce6" />
<br><br><br>

## malware analysis

malware có chức năng thu thập thông tin, bao như kiến trúc hệ điều hành, phiên bản framework, phiên bản hệ điều hành, v.v....

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 7 | TA0007 Discovery | T1057 Process Discovery | No-subtech | ProcessArch: x64 |
| 8 | TA0007 Discovery | T1083 File and Directory Discovery | No-subtech | CurrentDirectory: c:\windows\system32\inetsrv |
| 9 | TA0007 Discovery | T1087 Account Discovery | .001 Local Account | User: NT AUTHORITY\SYSTEM |
| 10 | TA0007 Discovery | T1087 Account Discovery | .002 Domain Account | Domain: DEV |

<img width="1419" height="367" alt="image" src="https://github.com/user-attachments/assets/808f8888-bdb4-4f43-a4c1-58c513fb7945" />
<img width="1129" height="590" alt="image" src="https://github.com/user-attachments/assets/f132c4ee-5802-4c2e-a479-72812ee290b6" />

<br>

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 11 | TA0005 Defense Evasion | T1055 Process Injection | .001 Dynamic-link Library Injection | DLL được inject vào tiến trình svchost.exe nhằm để tránh bị phát hiện bởi defense tools như AV, EDR, hoặc IDS. |
| 12 | TA0011 Command & Control | T1071 Application Layer Protocol | .001 Web Protocols | DLL thực hiện kết nối gửi nhận dữ liệu tới C2 thông qua các listener có sử dụng giao thức http, https cổng 80, 443 |

<img width="1365" height="692" alt="image" src="https://github.com/user-attachments/assets/3cf854e7-648e-4546-baab-731a0c44c5a8" />


## Tổng kết

| No | ID Tactic | ID Technique | ID Sub-Technique | Description | 
|----|-----------|--------------|------------------|-------------|
| 1 | TA0003 Persistence | T1505 Server Software Components | .003 Webshell | Các webshell được drop xuống Exchange server, mặc dù hầu hết đã bị obfuscated nhưng mục đích vẫn là duy trì quyền truy cập, kiểm soát từ xa |
| 2 | TA0005 Defense Evasion | T1027 Obfuscated Files or Information | .010 Command Obfuscation | Các webshell bị obfuscated mục đích làm rối mã, tránh bị detect bởi các tool defense,... |
| 3 | TA0003 Execution | T1059 Command and Scripting Interpreter | .003 Windows Command Shell | Lợi dụng cmd để thực hiện tải file độc hại, kiểm tra kết nối thông qua certutil có sẵn trên Windows |
| 4 | TA0003 Execution | T1047 Windows Management Instrumentation | No-subtech | Thực thi các file suspicious |
| 5 | TA0006 Credential Access | T1003 OS Credential Dumping | .001 LSASS Memory | Các file all.exe và dump.dll trích xuất thông tin tài khoản trên hệ thống, sử dụng rar.exe để nén các file dump và copy ra webroot của máy chủ Exchange |
| 6 | TTA0009 Collection | T1560 Archive Collected Data | .001 Archive via Utility | Sử dụng WinRAR rar.exe để nén các file dump sau quá trình thu thập thông tin xác thực ở trên |
| 7 | TA0007 Discovery | T1057 Process Discovery | No-subtech | ProcessArch: x64 |
| 8 | TA0007 Discovery | T1083 File and Directory Discovery | No-subtech | CurrentDirectory: c:\windows\system32\inetsrv |
| 9 | TA0007 Discovery | T1087 Account Discovery | .001 Local Account | User: NT AUTHORITY\SYSTEM |
| 10 | TA0007 Discovery | T1087 Account Discovery | .002 Domain Account | Domain: DEV |
| 11 | TA0005 Defense Evasion | T1055 Process Injection | .001 Dynamic-link Library Injection | DLL được inject vào tiến trình svchost.exe nhằm để tránh bị phát hiện bởi defense tools như AV, EDR, hoặc IDS. |
| 12 | TA0011 Command & Control | T1071 Application Layer Protocol | .001 Web Protocols | DLL thực hiện kết nối gửi nhận dữ liệu tới C2 thông qua các listener có sử dụng giao thức http, https cổng 80, 443 |
