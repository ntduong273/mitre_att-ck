# Tactics 1: Reconnaissance


## ID: TA0043

Giai đoạn attacker thăm dò, dò quét hệ thống của nạn nhân nhằm thu thập các thông tin hữu ích cho quá trình tấn công sau này.

Thông tin có thể là hạ tầng triển khai, nhân sự, dịch vụ cung cấp, công nghệ sử dụng, website, sản phẩm,....

Có thể bằng cách phương pháp **chủ động** _(như: scan mạng, port)_ hay **bị động** _(như: OSINT, Phishing)_.

Thông tin thu thập được tận dụng trong các giai đoạn khác của vòng đời tấn công, VD:
- Lên kế hoạch và thực hiện Initial Access (Tấn công vào đâu, cách thức nào, thời gian, địa điểm,...)
- Xác định phạm vi và ưu tiên các mục tiêu sau khi xâm nhập (Chỗ nào dễ, chỗ nào khó khai thác, chỗ nào chứa tài sản giá trị,...)
- ...

**Gồm 10 kỹ thuật:**
- T1595 Active Scanning: chủ động dùng các tools dò quét, thăm dò hạ tầng của victim thông qua lưu lượng mạng nhằm thu thập in4.
- T1592 Gather Victim Host Information
- T1589 Gather Victim Identity Information
- T1590 Gather Victim Network Information
- T1591 Gather Victim Org Information
- T1598 Phishing for Information
- T1597 Search Closed Sources
- T1596 Search Open Technical Databases
- T1593 Search Open Websites/Domains
- T1594 Search Victim-Owned Websites
