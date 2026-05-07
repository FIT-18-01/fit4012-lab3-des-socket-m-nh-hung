# Threat Model - Lab 3

## Thông tin nhóm
- Thành viên 1: Bùi Đình Mạnh - MSSV: 1871020379
- Thành viên 2: Phạm Ngọc Hùng - MSSV: 1871020265

## Assets
-Bản tin gốc (plaintext) cần được bảo mật trong quá trình truyền.
-DES key 8 byte và IV 8 byte — hiện đang được gửi dạng plaintext trên cùng luồng TCP với ciphertext.
-Kết nối TCP giữa Sender và Receiver — kênh truyền không được mã hoá hay xác thực.

## Attacker model
-Kẻ tấn công có thể ở trên cùng mạng LAN hoặc kiểm soát một node trung gian trên đường truyền. Kẻ tấn công có thể thực hiện nghe lén thụ động (passive eavesdropping) để thu thập toàn bộ lưu lượng TCP, hoặc thực hiện tấn công chủ động (active MITM) để chặn, sửa và chuyển tiếp gói tin. Kẻ tấn công không có quyền truy cập trực tiếp vào máy của Sender hoặc Receiver.

## Threats
1.Nghe lén key và IV: Vì key và IV được gửi plaintext trên cùng kênh TCP với ciphertext, kẻ tấn công chỉ cần capture một gói TCP duy nhất là có đủ thông tin để giải mã toàn bộ bản tin — DES-CBC hoàn toàn bị vô hiệu hoá.
2.Giả mạo Sender (spoofing): Không có cơ chế xác thực danh tính, kẻ tấn công có thể kết nối thẳng vào Receiver và gửi gói tin giả mạo với nội dung tuỳ ý.
3.Chỉnh sửa ciphertext (tampering): Kẻ tấn công có thể chặn và sửa các byte trong ciphertext. DES-CBC không có MAC hay chữ ký nên Receiver không thể phát hiện dữ liệu đã bị sửa — chỉ có thể nhận ra qua lỗi padding trong một số trường hợp.

## Mitigations
1.Dùng TLS/SSL cho toàn bộ kênh truyền: Bọc socket bằng TLS để mã hoá và xác thực kênh, ngăn nghe lén và MITM. Key trao đổi session được bảo vệ bởi handshake bất đối xứng.
2.Trao đổi key qua giao thức bất đối xứng: Dùng ECDH hoặc RSA để trao đổi key trước khi truyền dữ liệu, đảm bảo key không bao giờ đi qua kênh không bảo mật dạng plaintext.
3.Thêm MAC hoặc chữ ký số: Gắn HMAC-SHA256 vào mỗi packet để Receiver có thể xác minh tính toàn vẹn và nguồn gốc, phát hiện mọi hành vi tamper dù chỉ 1 bit.

## Residual risks
Ngay cả khi áp dụng TLS, nếu private key của server bị lộ và kẻ tấn công đã lưu lại toàn bộ lưu lượng trước đó, họ có thể giải mã hồi tố toàn bộ lịch sử (không có forward secrecy trừ khi dùng ECDHE). Ngoài ra DES với key 56-bit hiệu dụng đã bị phá bằng brute-force từ năm 1998 và không nên dùng trong bất kỳ hệ thống thực tế nào — cần thay bằng AES-256.
