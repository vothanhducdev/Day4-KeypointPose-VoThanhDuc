# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.72 khớp có v > 0 mỗi người
- Tổng: v=2 364 | v=1 92 | v=0 37

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 25 | 3 | 1 | 10% |
| 1 | left_eye | 22 | 6 | 1 | 21% |
| 2 | right_eye | 24 | 4 | 1 | 14% |
| 3 | left_ear | 15 | 12 | 2 | 41% |
| 4 | right_ear | 19 | 10 | 0 | 34% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 4 | 2 | 14% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 19 | 8 | 2 | 28% |
| 10 | right_wrist | 18 | 10 | 1 | 34% |
| 11 | left_hip | 25 | 3 | 1 | 10% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 19 | 6 | 4 | 21% |
| 14 | right_knee | 21 | 4 | 4 | 14% |
| 15 | left_ankle | 17 | 4 | 8 | 14% |
| 16 | right_ankle | 15 | 5 | 9 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
