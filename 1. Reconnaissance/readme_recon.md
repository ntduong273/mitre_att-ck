# Tactics 1: Reconnaissance


## ID: TA0043

Thăm dò, dò quét hệ thống của victim -> thu thập thông tin hữu ích cho quá trình tấn công.

Thông tin: hạ tầng triển khai, nhân sự, dịch vụ cung cấp, công nghệ sử dụng, website, sản phẩm,....

2 phương pháp: **chủ động** _(như: scan mạng, port)_ và **bị động** _(như: OSINT, Phishing)_.

in4 thu được, đc dùng trong các giai đoạn khác nhau của vòng đời tấn công, VD:
- Lên plan, thực hiện Initial Access (Tấn công vào đâu, cách thức nào, thời gian, địa điểm,...)
- Xác định phạm vi tc, ưu tiên mục tiêu nào khi xâm nhập (Chỗ nào dễ, chỗ nào khó khai thác, chỗ nào chứa tài sản giá trị,...)
- ...

**Gồm 10 kỹ thuật:**

- _**T1589 Gather Victim Identity Information:**_ gồm data cá nhân (tên nv, email,..), in4 nhạy cảm (in4 đăng nhập, cấu hình xác thực đa yếu tố - MFA).
- _**T1590 Gather Victim Network Information:**_ kiểu dải IP address, tên domain, các in4 cấu hình như cấu trúc liên kết và hoạt động.
- _**T1591 Gather Victim Org Information:**_ các bộ phận/phòng ban, hoạt động kinh doanh, vai trò, trách nhiệm của nviên chủ chốt,...
- _**T1592 Gather Victim Host Information:**_ dữ liệu quản trị (tên host, IP public được gán,..) in4 cấu hình (dùng OS gì, dùng tools gì,..).
- _**T1593 Search Open Websites/Domains:**_ in4 trên các site online, mxh, news, hoạt động kinh doanh, tuyển dụng, hợp đồng,...
- _**T1594 Search Victim-Owned Websites:**_ in4 trên website của nạn nhân, công ty
- _**T1595 Active Scanning:**_ chủ động dùng các tools dò quét, thăm dò hạ tầng của victim thông qua lưu lượng mạng nhằm thu thập in4.
- _**T1596 Search Open Technical Databases:**_ in4 từ các DB trực tuyến, storage công khai vd:đăng ký domain/certi, data/hiện vật mạng public.
- _**T1597 Search Closed Sources:**_ các nguồn, DB tư nhân uy tín, darkweb, chợ đen của tội phạm mạng
- _**T1598 Phishing for Information:**_ lừa victim lộ in4 nhạy cảm như password, id,... (khác với phishing thực thi mã độc)
