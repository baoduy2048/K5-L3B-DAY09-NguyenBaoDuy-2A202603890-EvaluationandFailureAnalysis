# Judge audit

Chạy: `python -m lab9.judge --csv <file> --audit 5`

| Tiêu chí | n | κ | Precision | Recall | Đạt FLOORS (κ≥0.6, P≥0.8, R≥0.7)? | Quyết định |
|---|---|---|---|---|---|---|
| tone_ok | 100 | 0.757 | 0.857 | 0.800 | Có | dùng |
| helpful | 100 | 0.375 | 1.000 | 0.300 | Không | loại |

## ≥5 case audit thủ công
| # | Case | Judge nói | Người nói | Ai đúng? | Ghi chú |
|---|---|---|---|---|---|
| 1 | H-008 | PASS | FAIL | Người | Reply dùng "em"/"nha", suồng sã và không phù hợp ngữ cảnh hỗ trợ; rubric cần bắt fail xưng hô và giọng không trang trọng |
| 2 | H-017 | PASS | FAIL | Người | Reply dùng "bạn" với khách, thiếu xưng hô phù hợp; rubric cần gồm quy tắc xưng hô theo kiểu khách hàng |
| 3 | H-029 | PASS | FAIL | Người | Câu "không cần nhắn thêm" mang giọng ra lệnh; rubric cần phân biệt thể hiện lịch sự hình thức với yêu cầu ngữ cảnh |
| 4 | H-042 | FAIL | PASS | Người | Reply ngắn nhưng đủ ý và có hình thức lễ phép cơ bản; rubric không được yêu cầu bắt buộc xin lỗi hoặc mở rộng văn bản |
| 5 | H-051 | FAIL | PASS | Người | "refund" được CS LogiX cho phép; rubric không được coi tiếng Anh trong ngữ cảnh này là vi phạm tone |

## Kết luận
- Judge `tone_ok` đạt FLOORS (κ≈0.757, P≈0.857, R≈0.800) và có thể dùng, nhưng vẫn cần sửa rubric để bắt xưng hô, giọng ra lệnh và không phạt sai mẫu ngắn hoặc từ được cho phép.
- Judge `helpful` bị loại vì κ≈0.375, Recall≈0.300; confusion là `{'tp': 9, 'fp': 0, 'fn': 21, 'tn': 70}` nên judge bỏ sót 21/30 câu người đánh FAIL, chủ yếu là sai số tiền.
- Rogan-Gladen: observed fail rate của tone là 0.28, corrected là 0.300.
- Rogan-Gladen của helpful cho kết quả 0.300, nhưng không được dùng để thay đổi cổng gate; judge này không đạt Recall hoặc κ tối thiểu.
- Rủi ro còn lại: judge chỉ được kiểm trên dữ liệu replay đã công khai; cần nhãn người mới trên bộ held-out thật trước khi dùng làm gate.
