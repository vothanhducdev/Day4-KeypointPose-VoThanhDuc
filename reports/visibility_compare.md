# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 342 | v=1 88 | v=0 29

So sánh với `gold\labels\train` (29 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 44% | 3% | 41 |
| 4 | right_ear | 37% | 14% | 23 |
| 9 | left_wrist | 30% | 7% | 23 |
| 11 | left_hip | 7% | 28% | 20 |
| 1 | left_eye | 22% | 3% | 19 |
| 10 | right_wrist | 37% | 21% | 16 |
| 2 | right_eye | 15% | 0% | 15 |
| 16 | right_ankle | 15% | 24% | 9 |
| 7 | left_elbow | 15% | 7% | 8 |
| 0 | nose | 11% | 3% | 8 |
| 8 | right_elbow | 11% | 3% | 8 |
| 15 | left_ankle | 15% | 17% | 2 |
| 13 | left_knee | 19% | 21% | 2 |
| 12 | right_hip | 22% | 21% | 2 |
| 14 | right_knee | 15% | 14% | 1 |
| 5 | left_shoulder | 7% | 7% | 1 |
| 6 | right_shoulder | 4% | 3% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
