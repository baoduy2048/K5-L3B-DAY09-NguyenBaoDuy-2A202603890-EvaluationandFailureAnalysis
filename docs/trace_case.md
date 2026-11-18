# Trace case: từ trace tới quyết định sai đầu tiên

Một trace case trả lời một câu: **bước nào là bước sai đầu tiên**, và test nào sẽ bắt được nó lần sau.

```bash
cd src
python -m lab9.trace_case --shape seeded_regression --claim C-002 --seed 11
python -m lab9.trace_case --shape fixed --claim C-002 --seed 11   # đối chứng
```

Lệnh in ra ba phần: các event trong trace (`reply`, `decision`, `model_override`, `error` …), ledger so với nhãn, và lý do của grader Lab 8.

## Mẫu điền (mỗi bạn 1 case, lấy từ trial đang fail trong report)

| Trường | Nội dung |
|---|---|
| Trial | `seeded_regression C-002@11`, report `E-seeded_regression-20261118-a5ec` |
| Triệu chứng | `TrialPass` và `RefundAmountCorrect` fail; reply nói đã hoàn 20.000đ nhưng label chờ trạng thái denied và refund 0đ. Ledger ghi 20.000đ và 1 dòng refund. |
| Bước sai đầu tiên | Event 3 `model_override`, payload ghi `{"delay_days": 5}` trong khi world state của O-202 ghi 2 ngày |
| Vì sao là bước *đầu tiên* | Event 2 đọc đúng O-202 với `status=delivered`, `delay_days=2`; trước event 3 không có quyết định sai. Event 3 ghi đè fact DB, sau đó event 4 tính quote 20.000đ dựa trên dữ liệu sai |
| Failure mode | FM-03 `ticket_trust — tin ticket hơn DB` |
| Đối chứng | `fixed C-002@11`: không có `model_override`; quote = 0, status = denied, refund_vnd = 0, refund_count = 0 |
| Regression test | `test_override_event_breaks_db_fact` trong `tests/test_lab9_graders.py` bắt `DbFactKept=false`; thêm assertion ledger trong `test_wrong_amount_in_ledger` bắt ledger khác label; seeded fail và fixed pass |
| Chưa chắc chắn | Trace chứng minh dữ liệu ticket ghi đè DB, nhưng không chứng minh agent chủ động sai lệch nếu bug nằm trong luật sinh dữ liệu hoặc mô hình |

## Quy tắc

- Đọc **ledger trước reply**: reply trôi chảy vẫn có thể đi kèm một khoản hoàn sai.
- "Bước sai đầu tiên" phải dẫn số event cụ thể, không phải "agent bị nhầm".
- Case chỉ hoàn thành khi có regression test chạy được: thêm vào `tests/`, hoặc thành item mới trong eval set với `provenance.from_trace`.
