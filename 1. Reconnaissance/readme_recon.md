<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/538fa9c1-2d8a-4aac-82c9-b55b6e9879b4" /># Tactics 1: Reconnaissance


## ID: TA0043

Giai đoạn attacker thăm dò, dò quét hệ thống của nạn nhân nhằm thu thập các thông tin hữu ích cho quá trình tấn công sau này.

Thông tin có thể là hạ tầng triển khai, nhân sự, dịch vụ cung cấp, công nghệ sử dụng, website, sản phẩm,....

Có thể bằng cách phương pháp **chủ động** _(như: scan mạng, port)_ hay **bị động** _(như: OSINT, Phishing)_.

Thông tin thu thập được tận dụng trong các giai đoạn khác của vòng đời tấn công, VD:
- Lên kế hoạch và thực hiện Initial Access (Tấn công vào đâu, cách thức nào, thời gian, địa điểm,...)
- Xác định phạm vi và ưu tiên các mục tiêu sau khi xâm nhập (Chỗ nào dễ, chỗ nào khó khai thác, chỗ nào chứa tài sản giá trị,...)
- ...

**Gồm 10 kỹ thuật:**
- _**T1595 Active Scanning:**_ chủ động dùng các tools dò quét, thăm dò hạ tầng của victim thông qua lưu lượng mạng nhằm thu thập in4. (Còn lại là bị động)
- _**T1592 Gather Victim Host Information:**_ dữ liệu quản trị (vd: tên host, IP public được gán,...), in4 cấu hình (vd: dùng OS gì, ngôn ngữ gì, dùng các công cụ gì,..).
- _**T1589 Gather Victim Identity Information:**_ gồm dữ liệu cá nhân (vd: tên nv, email,..), chi tiết nhạy cảm như thông tin đăng nhập, cấu hình xác thực đa yếu tố (MFA).
- _**T1590 Gather Victim Network Information:**_ kiểu dải địa chỉ IPs, tên domain hoặc các thông tin cấu hình như cấu trúc liên kết và hoạt động.
- _**T1591 Gather Victim Org Information:**_ 
- _**T1598 Phishing for Information:**_
- _**T1597 Search Closed Sources:**_
- _**T1596 Search Open Technical Databases:**_
- _**T1593 Search Open Websites/Domains:**_
- _**T1594 Search Victim-Owned Websites:**_
