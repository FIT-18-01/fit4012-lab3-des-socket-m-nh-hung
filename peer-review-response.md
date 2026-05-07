# Peer Review Response

## Thông tin nhóm
- Thành viên 1: Bùi Đình Mạnh - MSSV: 1871020379
- Thành viên 2: Phạm Ngọc Hùng - MSSV: 1871020265

## Thành viên 1 góp ý cho thành viên 2
Phạm Ngọc Hùng: Phần receiver.py xử lý đúng luồng nhận header và ciphertext, code rõ ràng và dễ đọc. Report mô tả đầy đủ cách làm và kết quả. Góp ý thêm nên bổ sung log rõ hơn ở phần receiver để dễ đối chiếu khi demo, ví dụ in thêm key và IV nhận được để xác nhận khớp với sender.

## Thành viên 2 góp ý cho thành viên 1
Bùi Đình Mạnh: Phần tests viết đầy đủ, có cả negative test cho tamper và wrong key đúng yêu cầu CI. Threat model phân tích đúng điểm yếu chính của hệ thống. Góp ý thêm phần mitigations nên giải thích ngắn gọn tại sao ECDH tốt hơn RSA trong trường hợp này để phần trình bày thêm thuyết phục khi giảng viên hỏi chéo.

## Nhóm đã sửa gì sau góp ý
Sau khi review chéo, nhóm đã bổ sung thêm ghi chú trong threat model về lý do ưu tiên ECDHE so với RSA thuần để đảm bảo forward secrecy. Đồng thời đã kiểm tra lại toàn bộ log trong thư mục logs/ để đảm bảo cả sender.log và receiver.log đều có nội dung đầy đủ, khớp nhau về key và IV. Ngoài ra đã rà soát lại README để đảm bảo không còn dòng TODO nào sót lại trước khi nộp bài.