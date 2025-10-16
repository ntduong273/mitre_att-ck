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


## Detection 

