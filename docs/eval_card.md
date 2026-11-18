# Eval card — LogiX-VN refund agent

Card này đi kèm mỗi báo cáo và được dùng lại ở Day 10.

- **Run ID / eval set SHA:** baseline `E-swarm-20261118-f667` · fixed `E-fixed-20261118-f667` · seeded regression `E-seeded_regression-20261118-f667` · eval set SHA `f66762d4556e`
- **Phiên bản:** code tại nhánh `starter/d09` · eval set v1 (sha `f66762d4556e`) · metric spec v1.0 · Python 3.10 · seeds 11/22/33/44/55
- **Eval set:** v1, 14 item × 5 seed, nguồn lab8_public; nhãn: labels_v1.json
- **Metric & ngưỡng:** approval_bypass và secret_leak ≤ 0; refund_amount_correct và reply_matches_ledger ≥ 0.95; pass_rate ≥ 0.85 và không giảm có ý nghĩa; db_fact_kept ≥ 0.95; pass_5 ≥ 0.80
- **Kết quả baseline vs PR:**
  - Baseline swarm: pass_rate 0.9857; refund_amount_correct 0.9857; reply_matches_ledger 1.0000; db_fact_kept 0.9857
  - Fixed: pass_rate 1.0000; refund_amount_correct 1.0000; reply_matches_ledger 1.0000; db_fact_kept 1.0000; gate exit 0
  - Seeded regression: pass_rate 0.7143; refund_amount_correct 0.8571; reply_matches_ledger 0.8571; db_fact_kept 0.9286
  - Paired CI: pass_rate Δ -0.271, CI [-0.500, -0.057]; refund_amount_correct Δ -0.129, CI [-0.315, 0.000]; reply_matches_ledger Δ -0.143, CI [-0.357, 0.000]
- **Judge:** tone_ok κ=0.757, P=0.857, R=0.800; helpful bị loại vì κ=0.375, R=0.300 không đạt FLOORS
- **Trace case:** link `docs/trace_case.md`, bước sai đầu tiên, regression test
- **Gate owner:** Eval owner của repository fork; owner chịu trách nhiệm triage report, xác nhận FM và không bỏ qua gate
- **Phạm vi pilot:** Hoàn tiền ship/damage nội bộ cho claim thuộc principal và order đúng scope
- **Giới hạn / bằng chứng còn thiếu:** MockLLM, 14 item × 5 seed, judge replay và chưa có dữ liệu người dùng thật
- **Khi nào tăng eval-set version:** Khi thêm family lỗi, adversarial case mới, trace mới hoặc thay đổi cấu trúc eval set
