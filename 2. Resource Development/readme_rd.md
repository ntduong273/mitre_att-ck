# Tactics 2: Resource Development

## ID: TA0042

thiết lập, phát triển tài nguyên để support cho quá trình tấn công

resoure có thể là hạ tầng, tài khoản, dịch vụ, kĩ năng, khả năng. Có thể tự tạo, mua hoặc xâm nhập/đánh cắp các resource có ích. VD: mua domain làm C2, tạo email để phishing,...

**Có 8 techniques:**

- _**T1650 Acquire Access:**_ mua/chiếm quyền truy cập đã tồn tại vào hệ thống từ các bên tội phạm khác
- _**T1583 Acquire Infrastructure:**_ mua/thuê/mượn/chiếm hạ tầng để tấn công (server vlý/cloud, domain, web service, botnet)
- _**T1586 Compromise Accounts:**_ chiếm lấy các acc sẵn có, quyền cao trong hệ thống, tiết kiệm tgian, né bị check
- _**T1584 Compromise Infrastructure:**_ chiếm lấy hạ tầng của 1 bên khác để tấn công thay vì thuê/mua, tạo botnet
- _**T1587 Develop Capabilities:**_  tự tạo, phát triển capabilities (malware, payload, exploit), nâng cao skill
- _**T1585 Establish Accounts:**_ tạo, nuôi acc, dựng nhân dạng (persona) như một acc uy tín, hợp pháp 
- _**T1588 Obtain Capabilities:**_ mua/tải bên ngoài, chiếm đoạt capbilities đã có (mua exploit kit, tải open-source C2)
- _**T1608 Stage Capabilities:**_ cbị, đặt sẵn/cất trữ capabilities ở nơi tiện trước khi tấn công(upload payloads lên hosting,..)
