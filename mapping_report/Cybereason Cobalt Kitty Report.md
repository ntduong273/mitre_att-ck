# Cybereason Cobalt Kitty Report

## Chi tiết vòng đời tấn công

Là một chiến dịch APT có 5 phase, nhắm vào các công ty toàn cầu.

-----------------------------------------------------------------------------------------------------------------------------------

## Phase 1: Penetration

**Tactic theo MITRE ATT&CK: TA0043 - Recon, TA0042 - rd, TA0001 - initial access, TA0002 - exe**

| ID Tactic | ID Technique | ID Sub-Technique | Description | 
|-----------|--------------|------------------|-------------|
| TA0001 Initial Access | T1566 Phishing | .001 Spearphishing Attachment | Nhóm tấn công sử dụng phương pháp social engineering (kỹ nghệ xã hội), cụ thể là kiểu tấn công spear-phishing email, gửi một email giả mạo có 2 mục đích:<br>- Link tới 1 site độc hại, download 1 fake Flash Installer có chứa Cobalt Strike Beacon.<br>- Có 1 file Word chứa các macro độc hại sẽ download các Cobalt Strike payloads. |

Giải thích cụ thể:

<img width="1405" height="574" alt="image" src="https://github.com/user-attachments/assets/a4266e9f-065a-4ff2-85c3-80990c47aec0" />



| ID Tactic | ID Technique | ID Sub-Technique | Description | 
|-----------|--------------|------------------|-------------|
| TA0002 Execution | T1059 Command and Scripting Interpreter | .001 PowerShell | Tiến trình <code>install_flashplayers.exe</code> có 7 tiến trình con, cụ thể nó có gọi 1 tiến trình <code>powershell.exe</code> lên để thực thi code (Tiêm tiến trình). |
| TA0002 Execution | T1059 Command and Scripting Interpreter | .003 Windows Command Shell | Đầu tiên, fake Flash installer nó sẽ gọi 1 tiến trình <code>cmd.exe</code> lên để thực hiện scripts, chạy <code>install_flashplayers.exe</code>. |
| TA0005 Defense Evasion | T1055 Process Injection | .001 Dynamic-link Library Injection | Sử dụng <code>powershell.exe</code> để tiến hành tiêm tiến trình <code>install_flashplayers.exe</code> vào một tiến trình hệ thống hợp pháp là <code>rundll32.exe</code>. Mục đích để che giấu hoạt động độc hại bằng cách tận dụng tiến trình hợp pháp, giúp tránh bị phát hiện bởi phần mềm diệt virus hoặc EDR|

Giải thích cụ thể:

<img width="1408" height="848" alt="image" src="https://github.com/user-attachments/assets/645cb020-0deb-4603-9397-d3d5344ffc4e" />


-----------------------------------------------------------------------------------------------------------------------------------

## Phase 2: Foothold and persistence


## Phase 3: Command & control and data exfiltration


## Phase 4: Internal reconnaissance


## Phase 5: Lateral movement

