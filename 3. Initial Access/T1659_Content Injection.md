# Content Injection

Xâm nhập, duy trì = cách chèn payload độc hại vào system thông qua lưu lượng mạng trực tuyến. 

Thay vì dụ victims truy cập vào 1 payload độc hại trên 1 website(Drive-by Compromise) thì có thể initial access thông qua các **kênh dữ liệu trực tuyến bị xâm nhập** - nơi attacker thao túng, kiểm soát lưu lượng, chặn/chèn nội dung độc hại.

Chèn nội dung vào hệ thống nạn nhân theo nhiều cách khác nhau, bao gồm:

- Từ giữa (From the middle): khi attacker đứng giữa luồng giao tiếp hợp lệ giữa client và server trực tuyến.
- Từ bên cạnh (From the side): khi payload độc hại được chèn vào 1 fake response, gửi nhanh hơn response hợp lệ từ server thật tới client.

=> the result of compromised upstream communication channel: can thiệp vào các kênh đường truyền mà lưu lượng Internet của victims đi qua, chặn bắt, sửa đổi gói tin qua lại.

## Procedure Examples (Ví dụ thủ thuật / Quy trình thực hiện)

| ID | Name | Description |
|----|------|-------------|
| S1088 | Disco | |
| G1019 | MoustachedBouncer | |

## Mitigations (Biện pháp giảm thiểu / Phòng ngừa)

## Detection
