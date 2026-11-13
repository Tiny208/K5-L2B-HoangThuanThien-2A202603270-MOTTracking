# Báo cáo cá nhân — NguyenVanDemo-DEMO2024

> **FILE MẪU ĐÃ ĐIỀN** — chỉ để xem format. Không nộp file này.
> Đề B (MSSV demo `DEMO2024`). Số frame ghi theo CVAT (0–149).
> Ảnh che khuất ở mục 3 là đường dẫn mẫu; bạn phải chụp ảnh thật của mình.

Đoạn video: MOT17-09, 150 frame (frame 1–150), nguồn MOTChallenge, giấy phép CC BY-NC-SA 3.0.

## 0. Thông tin học viên

- Họ tên: Nguyen Van Demo
- MSSV: DEMO2024
- Mã đề (A/B/C): **B**
- Email: demo@example.com
- Link repository: https://github.com/example/K5-L2-DAY05-NguyenVanDemo-DEMO2024-MOTTracking
- Giờ bắt đầu: 13:05
- Giờ kết thúc: 16:40

## 1. Xem trước đoạn video (CP0)

Ghi **trước khi vẽ**, mỗi người một dòng.

| Mã | Frame xuất hiện | Frame cuối thấy được | Rẽ / dừng / đổi tốc độ ở frame | Bị che ở frame | Ghi chú |
|---|---|---|---|---|---|
| P1 | 0 | 149 | ~35–42 (đổi hướng sang phải), ~88 (chậm lại) | ~70–95 (đi sau người khác / cột) | Người mục tiêu CP2; theo dõi kỹ đoạn giữa |
| P2 | 0 | 120 | ~55 (rẽ nhẹ), ~100 (ra xa) | ít | Ra khỏi khung trước 149 → sẽ bật `outside` |
| P3 | 0 | 149 | ~20–28 (đổi tốc độ), ~110 (lại gần) | không rõ | Box to dần khi lại gần camera |
| P4 | 40 | 149 | ~75 (đổi hướng), ~130 (dừng ngắn) | ~80–86 (che một phần) | Xuất hiện muộn (frame ~40) |
| P5 | 2 | 149 | ~48 (rẽ), ~140 (gần mép khung) | ~60–68 | Dễ lẫn với P3 lúc đi sát |

## 2. Keyframe và thời gian (CP1, CP4)

Số keyframe lấy từ kết quả `src/check_submission.py`.

| Mã | Số keyframe | Keyframe ở frame | Số frame có mặt | Keyframe / 100 frame | Thời gian (phút) | Chỗ khó |
|---|---|---|---|---|---|---|
| P1 | 14 | 0, 18, 35, 42, 55, 69, 70, 95, 96, 110, 125, 140, 145, 149 | 130 (có đoạn outside) | 10,8 | 18 | Đoạn che 70–95; giữ cùng ID |
| P2 | 11 | 0, 15, 40, 55, 70, 85, 100, 110, 115, 118, 120 | 121 | 9,1 | 12 | Outside từ frame 121 |
| P3 | 12 | 0, 12, 28, 45, 60, 80, 95, 110, 120, 130, 140, 149 | 150 | 8,0 | 11 | Đổi kích thước khi lại gần |
| P4 | 10 | 40, 50, 65, 75, 86, 100, 115, 130, 140, 149 | 110 | 9,1 | 10 | Xuất hiện muộn; che nhẹ ~80 |
| P5 | 13 | 2, 10, 25, 48, 60, 68, 85, 100, 115, 130, 140, 145, 149 | 148 | 8,8 | 14 | Đi sát P3 quanh frame 48–60 |
| **Tổng** | **60** | | **659** | | **65** | |

Keyframe / 100 frame của cả bài = tổng keyframe ÷ tổng số frame có mặt × 100 = 60 ÷ 659 × 100 ≈ **9,1**

Bạn đặt keyframe nhiều nhất ở người nào? Vì sao (người đó rẽ, dừng, lại gần camera hay bị che)?

P1 — vì có đoạn bị che dài và đổi hướng trước/sau chỗ che; cần keyframe dày quanh 69–70 và 95–96 để nội suy không cắt góc và không nhảy box khi tắt `outside`.

## 3. Đoạn bị che (CP2)

Người bị che: `P1`

| Mốc | Frame |
|---|---:|
| Bắt đầu bị che một phần (bật `occluded`) | 70 |
| Mất hẳn (bật `outside`) | 78 |
| Hiện lại (tắt `outside`) | 95 |
| Thấy rõ trở lại (tắt `occluded`) | 98 |

- Thời gian khuất hẳn: **17** frame = **0,57** giây (17/30).
- Quyết định: **giữ cùng ID**.
- Lý do (trả lời 3 câu: khuất bao lâu so với 60 frame, ngoại hình có khớp, chỗ hiện lại có đúng hướng và tốc độ):

1. Khuất hẳn 17 frame ≪ 60 frame (ngưỡng 2 giây của lớp) → đủ điều kiện giữ cùng track.  
2. Áo/quần và dáng lúc hiện lại khớp P1 trước khi che; không nhầm với người đi ngang.  
3. Hiện lại đúng phía đường đi cũ, tốc độ tương đương → tắt `outside` ở 95, chỉnh box (keyframe mới), không tạo track mới.

Ảnh chụp màn hình:

![Trước khi che](images/che_1_truoc.png)
![Giữa đoạn che, thấy cờ trên panel object](images/che_2_giua.png)
![Sau khi hiện lại, cùng ID](images/che_3_sau.png)

*(Trong bản nộp thật: đặt 3 ảnh đã chụp vào `report/individual/images/` với đúng tên trên.)*

## 4. Tự review ID (CP3)

Lượt 1 theo người (đi ngược từ frame 149 về 0 trên CVAT), lượt 2 theo cảnh (nhảy 10 frame). Ít nhất 3 dòng; không có lỗi thì ghi một dòng "đã kiểm hai lượt, không lỗi".

Loại lỗi dùng đúng một trong 7 mã: `id_switch`, `fragmentation`, `lost`, `merge`, `false_track`, `drift`, `flag`.

| Frame | Mã người | Loại lỗi | Mô tả một câu | Phát hiện ở lượt | Đã sửa? |
|---:|---|---|---|---|---|
| 52 | P5 | `drift` | Box nội suy lệch nửa thân giữa keyframe 48 và 60 | 1 (theo người) | Có — thêm keyframe 52 |
| 82 | P1 | `flag` | Đã mất hẳn nhưng quên bật `outside` sớm, vẫn thấy box mờ | 1 | Có — bật `outside` từ 78 |
| 48–55 | P3/P5 | `id_switch` (nguy cơ) | Hai người đi sát, ID P5 suýt nhảy sang P3 khi kéo box | 2 (theo cảnh) | Có — split rồi merge lại đúng người; thêm keyframe |

Tổng hợp số lỗi theo loại:

| `id_switch` | `fragmentation` | `lost` | `merge` | `false_track` | `drift` | `flag` |
|---:|---:|---:|---:|---:|---:|---:|
| 1 | 0 | 0 | 0 | 0 | 1 | 1 |

Lỗi nào chỉ lượt 2 (theo cảnh) mới thấy, lượt 1 (theo người) không thấy? Vì sao?

Nguy cơ `id_switch` P3/P5 quanh 48–55: khi review từng người một, mỗi box nhìn vẫn “khớp người đang theo”, nhưng nhìn cùng lúc cả 5 box mới thấy ID suýt đổi chỗ khi hai người cắt nhau.

## 5. File xuất và kết quả tự kiểm (CP4)

| Kiểm tra | Kết quả |
|---|---|
| Tên file MOT 1.1 | `NguyenVanDemo_mot.zip` |
| Tên file CVAT for video 1.1 | `NguyenVanDemo_cvat_video.zip` |
| Số track ID khác nhau trong `gt/gt.txt` (phải là 5) | 5 |
| Frame `outside` không có dòng trong `gt.txt` (đúng / sai) | đúng |
| Job đã completed trên CVAT máy cá nhân | có |

Dán phần tóm tắt in ra từ `src/check_submission.py`:

```text
=== Tóm tắt check_submission ===
Số track ID: 5
P1  frame có mặt: 0–77, 95–149  | keyframe: 14  | kf/100f: 10.8
P2  frame có mặt: 0–120         | keyframe: 11  | kf/100f: 9.1
P3  frame có mặt: 0–149         | keyframe: 12  | kf/100f: 8.0
P4  frame có mặt: 40–149        | keyframe: 10  | kf/100f: 9.1
P5  frame có mặt: 2–149         | keyframe: 13  | kf/100f: 8.8
[CẢNH BÁO] không có
[LỖI] không có
Mã thoát: 0
```

*(Số liệu trên là minh họa cho mẫu; bản nộp thật dán nguyên output script của bạn.)*

## 6. Ba câu kết

1. Nếu được gán lại, bạn sẽ đặt keyframe khác ở đâu? Vì sao?

   Sẽ dày thêm quanh frame 48–55 (P3/P5 đi sát) và ngay trước/sau chỗ che của P1 (69 và 96), vì đó là chỗ nội suy tuyến tính dễ cắt góc và dễ kéo nhầm ID. Đoạn đi thẳng đều của P3 sẽ để thưa hơn để khỏi rung box.

2. Có chỗ nào bạn không chắc "cùng ID hay ID mới"? Bạn đã quyết định và ghi chú thế nào?

   P1 lúc hiện lại ở frame 95: ban đầu hơi nghi vì bị che gần 0,6 s và có người khác ngang qua. Đã áp dụng 3 câu của guideline (≤ 60 frame, ngoại hình khớp, hướng/tốc độ khớp) → giữ cùng ID; ghi chú trên object và trong mục 3.

3. Một lỗi ID trong job của bạn (nếu không còn sót, chọn lỗi đã sửa) có thể làm xe tự lái dự đoán sai chuyển động của người đó như thế nào? Lỗi đó ảnh hưởng MOTA hay IDF1 nhiều hơn?

   Nếu `id_switch` P5→P3 không sửa, vận tốc ước từ nhãn sẽ nhảy vọt giữa hai quỹ đạo (như người “dịch tức thì”), planner có thể phanh/ánh sai. `id_switch` phạt nặng **IDF1** (sai liên kết danh tính theo thời gian); MOTA cũng giảm vì IDSW nhưng thường IDF1 phản ánh rõ hơn lỗi giữ ID.

## Bonus (nếu làm)

### B1 — Hai người đi sát

Không làm (hết thời gian sau CP4).

### B2 — So cách đặt keyframe

Không làm.

### B3 — So với tracker có sẵn

Không làm.

## 7. Tài liệu tham khảo và công cụ AI đã dùng

- `docs/GUIDELINE.md`, `docs/CVAT_CHEATSHEET.md`, `docs/MOT_FORMAT.md`, `CHECKPOINTS.md`
- CVAT Community local (`localhost:8080`) + `src/setup_local_lab.py`, `src/check_submission.py`
- Công cụ AI: Cursor (hỏi format báo cáo / làm rõ quy ước keyframe và `outside`); không nhờ AI gán nhãn hộ trên CVAT
