# Tactics 3: Initial Access - Xâm nhập ban đầu

## ID: TA0001

Dùng nhiều đường khác nhau để đạt được 1 chỗ đứng đầu tiên trong hệ thống. Kỹ thuật phổ biến: spear-phishing có mục tiêu, khai thác lỗ hổng trên public web server.

Chỗ đứng ban đầu này có thể được duy trì lâu dài hoặc tạm thời (cấu hình bị đổi).

**Có 11 techniques:**

- _**T1659 Content Injection:**_ truy cập, duy trì = chèn payload độc vào lưu lượng mạng, thông qua các kênh chuyển data bị xâm phạm(DNS, Proxy, CDN)
- _**T1189 Drive-by Compromise:**_ lừa nạn nhân tới payload độc hại được lưu trên một trang web bị xâm phạm >< content injection
- _**T1190 Exploit Public-Facing Application:**_ khai thác điểm yếu trên host/system tiếp xúc với Internet (lỗi phần mềm, cấu hình sai)
- _**T1133 External Remote Services:**_ lợi dụng các services này để truy cập/duy trì tồn tại (VPN-bên ngoài kết nối tới tài nguyên mạng nội bộ)
- _**T1200 Hardware Additions:**_ thêm các phụ kiện, pcứng, tbi mạng vào hệ thống/mạng nội bộ -> làm vector để xâm nhập
- _**T1566 Phishing:**_ các social engineering thực hiện qua mạng, 2 kiểu:nhắm vào mục tiêu cụ thể(spearphishing) và hàng loạt(đại trà, random)
- _**T1091 Replication Through Removable Media:**_ sử dụng ptien lưu trữ rời chứa malware cắm vào hthong, lợi dụng autorun để chạy malware
- _**T1195 Supply Chain Compromise:**_ thao túng sphẩm/cơ chế giao nhận sp trước khi tới end-user nhằm xâm phạm dữ liệu/hệ thống
- _**T1199 Trusted Relationship:**_ xâm nhập/lợi dụng mqh của các tổ chức có quyền, truy cập tới victims mà không bị check
- _**T1078 Valid Accounts:**_ lạm dụng in4 đăng nhập của các tkhoản hiện có để đạt Initial Access, Persistence,...
- _**T1669 WiFi-Networks:**_ truy cập vào mạng không dây của mục tiêu, lợi dụng Wi-Fi mở/xâm phạm Wi-Fi được bảo mật
