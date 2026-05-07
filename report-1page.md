# Report 1 page - Lab 3

## Thông tin nhóm
- Thành viên 1: Bùi Đình Mạnh - MSSV: 1871020379
- Thành viên 2: Phạm Ngọc Hùng - MSSV: 1871020265

## Mục tiêu
Bài lab xây dựng hệ thống gửi và nhận bản tin mã hoá DES-CBC qua TCP socket gồm hai tiến trình Sender và Receiver. Mục tiêu là hiểu rõ luồng hoạt động thực tế của mã hoá đối xứng: Sender tạo key và IV ngẫu nhiên, mã hoá bản tin bằng DES-CBC với padding PKCS#7, đóng gói thành packet có header rõ ràng rồi truyền qua socket. Receiver nhận đúng thứ tự header rồi giải mã để khôi phục bản tin gốc. Bài lab cũng yêu cầu nhận thức về các hạn chế bảo mật của thiết kế này trong thực tế.

## Phân công thực hiện
Bùi Đình Mạnh phụ trách: sender.py, viết và chạy tests, lưu log minh chứng, threat-model-1page.md.
Phạm Ngọc Hùng phụ trách: receiver.py, report-1page.md, peer-review-response.md.
Phần làm chung: des_socket_utils.py, README.md, chạy demo thật và review chéo lẫn nhau.

## Cách làm
Sender đọc bản tin từ biến môi trường hoặc bàn phím, tạo key 8 byte và IV 8 byte ngẫu nhiên, mã hoá bằng DES-CBC với padding PKCS#7, sau đó đóng gói theo thứ tự key + IV + length (4 byte big-endian) + ciphertext rồi gửi qua TCP socket. Receiver bind socket, chấp nhận kết nối, đọc đúng 20 byte header để lấy key, IV và độ dài ciphertext, tiếp tục đọc đúng số byte ciphertext, giải mã và in bản tin gốc. Kiểm thử được thực hiện bằng pytest với 6 test case bao gồm roundtrip padding, kiểm tra header packet, test tamper ciphertext và test wrong key.

## Kết quả
Hệ thống chạy thành công trên local với bản tin "Xin chao FIT4012". Sender in ra key hex, IV hex và ciphertext hex. Receiver giải mã đúng và in ra bản tin gốc. Toàn bộ 6/6 test case pytest đều PASSED trong 0.23s, bao gồm 2 negative test là tamper ciphertext và wrong key. Log chạy thật được lưu tại logs/sender.log và logs/receiver.log.

## Kết luận
Bài lab cho thấy DES-CBC hoạt động đúng về mặt kỹ thuật: padding PKCS#7 đảm bảo dữ liệu luôn là bội số của 8 byte, IV ngẫu nhiên giúp cùng một bản tin cho ra ciphertext khác nhau mỗi lần. Tuy nhiên về bảo mật, việc gửi key và IV dạng plaintext trên cùng kênh TCP là điểm yếu nghiêm trọng: bất kỳ ai nghe lén đều có thể giải mã toàn bộ bản tin. Trong thực tế cần dùng TLS hoặc trao đổi key qua giao thức bất đối xứng như ECDH để bảo vệ kênh truyền.
