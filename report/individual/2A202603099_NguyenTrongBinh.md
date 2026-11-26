# Báo cáo Day 14 — Nguyễn Trọng Bình — 2A202603099

> Sao chép file này thành `report/individual/<MSSV>_<HoTen>.md` (không dấu), ví dụ `2A20260000_NguyenVanAn.md`.

- Mã học viên dùng với `--student`: `099`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: Tăng `score_thr` lên mức vừa phải (khoảng `0.3` đến `0.4`) kết hợp đặt `min_points = 5`.
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Trên 3 frame tập A (`s03f56`, `s04f02`, `s05f27`), mô hình dự đoán tới 88 box trong khi nhãn chuẩn chỉ có 57 box — lượng **box thừa rất nhiều** (phải xóa 39 box, tốn $39 \times 0.3 = 11.7$ điểm chi phí, chiếm hơn một nửa tổng chi phí `20.2`; riêng frame `s03f56` dư 22 box, phần lớn là `Cyclist` và `Car` nhận nhầm vào bụi cây, cột điện hoặc nhiễu nền ít điểm LiDAR). Ngược lại, số box sót chỉ có 8 box ($8 \times 1.0 = 8.0$ điểm). Vì vậy, nâng `score_thr` lên `0.3`–`0.4` và lọc bỏ các box có dưới `5` điểm LiDAR sẽ cắt giảm mạnh các box nhiễu để hạ chi phí xóa, đồng thời tránh đặt ngưỡng quá gắt (như `0.5`) gây lọc nhầm xe thật ở xa (mỗi box sót bị phạt `1.0`, đắt gấp hơn 3 lần xóa).

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | 39 | 8 | 1 | 20.2 |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_099/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_099/frames_B.txt --config d14_099/best_config.json --gt d14_099/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...