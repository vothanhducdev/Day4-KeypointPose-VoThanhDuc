# Đánh giá chéo bài bạn cùng nhóm (Review Partner)

Người gán: Nguyễn Văn A (Nhóm 04)   |   Người kiểm: Học viên Lab 4   |   Ngày: 16/09/2026

## 1. Reviewer Checklist

| # | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | :---: | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ Đạt | 20 ảnh đủ 27 skeleton, mỗi skeleton đủ 17 khớp |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☒ Chưa | Phát hiện lỗi chéo xương ở `train_02.jpg` (hông) |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ Đạt | Không có hiện tượng nhầm người (`nham_nguoi`) |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☒ Chưa | Có 2 khớp bị che ở `train_07.jpg` nhưng bạn lại tick `v = 0` |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☒ Chưa | `train_04.jpg` có khớp ra ngoài ảnh nhưng bạn vẫn để `v = 2` |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ Đạt | Không dùng phím tắt `h` |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ Đạt | File json export chuẩn cấu trúc 17 điểm $\times$ 3 toạ độ |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ Đạt | Đủ 1 class + 4 bbox + 51 keypoints |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ Đạt | Đã sinh và so sánh độ lệch %v=1 |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ Đạt | Đã cập nhật bổ sung luật sau thảo luận |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☒ Chưa | Còn 2 lỗi toạ độ ngoài biên ở `train_04.txt` cần sửa về `v = 0` |

---

## 2. Chi tiết các lỗi tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| :--- | :---: | :--- | :--- | :--- |
| `train_02.jpg` | 1 | `left_hip`, `right_hip`, gối, mắt cá | **Đảo trái/phải (`dao_trai_phai`)**: Chi dưới bị đổi ngược bên trái thành bên phải so với thân trên | Tráo lại vị trí giữa `left_hip` $\leftrightarrow$ `right_hip`, `left_knee` $\leftrightarrow$ `right_knee`, `left_ankle` $\leftrightarrow$ `right_ankle` |
| `train_04.jpg` | 1 | `left_ankle`, `right_ankle` | **Toạ độ ngoài ảnh nhưng để `v = 2`**: Bàn chân vượt ra ngoài đáy ảnh | Chuyển cờ về `v = 0` (Outside), toạ độ để `0.0 0.0 0` |
| `train_07.jpg` | 1 | `left_elbow`, `left_wrist` | **Xoá khớp bị che (`xoa_khop_bi_che`)**: Cánh tay trái bị thân che khuất nhưng lại tick `v = 0` | Đặt chấm ước lượng theo trục cánh tay và đổi cờ sang `v = 1` (Occluded) |

---

## 3. Kết luận đánh giá

- **Lỗi lặp đi lặp lại nhiều nhất của bài này**: Lỗi nhầm lẫn giữa cờ `v = 0` (Outside - ra ngoài khung) và `v = 1` (Occluded - bị che trong khung), cùng với việc chưa chú ý toạ độ vượt mép biên ảnh.
- **Nó là lỗi thao tác hay lỗi guideline chưa rõ**: Đây chủ yếu là **lỗi thao tác** khi thao tác phím tắt trong CVAT (bấm nhầm giữa tick Occluded và Outside), kết hợp với việc chưa chạy script kiểm tra tự động `check_pose_labels.py` trước khi nộp. Sau khi bổ sung guideline và thống nhất luật, bạn đã nắm vững cách sửa.
