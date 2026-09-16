# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Học viên Lab 4   Nhóm: Nhóm 04   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 364 / 92 / 37 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 41% (12/29 khớp bị che)
2. `right_ear`: 34% (10/29 khớp bị che)
3. `right_wrist`: 34% (10/29 khớp bị che) và `left_wrist`: 28% (8/29 khớp bị che)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**
Không hoàn toàn. Cần phân biệt rõ giữa khớp "hay bị che khuất" và khớp "khó xác định vị trí giải phẫu":
- Khớp tai (`left_ear`, `right_ear`) có tỉ lệ `%v=1` cao nhất vì người trong ảnh thường nghiêng mặt, đội mũ hoặc để tóc dài che phủ vành tai, nhưng việc ước lượng vị trí tai lại rất dễ dàng do tai luôn nằm đối xứng ngang tầm mắt và sống mũi.
- Ngược lại, khớp khó gán nhất trên thực tế là khớp hông (`left_hip`, `right_hip`). Mặc dù `%v=1` của hông không cao bằng tai, nhưng hông hầu như không bao giờ lộ ra bề mặt khi người mặc quần áo rộng, bắt buộc người gán nhãn phải suy luận dựa trên trục xương đùi và vị trí thắt lưng.

---

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.7240 | 0.8940 |
| OKS@0.50 | 0.8148 | 1.0000 |
| OKS@0.75 | 0.6296 | 0.9655 |
| Lỗi `dao_trai_phai` | 3 | 0 |
| Lỗi `nham_nguoi` | 2 | 0 |
| Lỗi `xoa_khop_bi_che` | 4 | 0 |

**Tôi đã sửa gì giữa hai lần chạy:**

- `train_02.jpg`, người thứ 1: Sửa lỗi đảo trái/phải (`dao_trai_phai`) ở nửa dưới cơ thể. Hoán đổi lại vị trí của 3 cặp khớp: `left_hip` $\leftrightarrow$ `right_hip`, `left_knee` $\leftrightarrow$ `right_knee`, `left_ankle` $\leftrightarrow$ `right_ankle`.
- `train_03.jpg`, người thứ 2 & `train_17.jpg`, người thứ 1: Sửa lỗi đảo trái/phải (`dao_trai_phai`) ở mắt. Hoán đổi lại cặp `left_eye` $\leftrightarrow$ `right_eye` để đồng hướng giải phẫu với cơ thể.
- `train_04.jpg`: Sửa lỗi nhầm người (`nham_nguoi`) ở cổ tay trái (`left_wrist`) của người 1 và người 2, đưa điểm về đúng cơ thể tương ứng; đồng thời chuyển các khớp chân ngoài mép ảnh về `v = 0`.
- `train_13.jpg`: Bổ sung 2 người còn thiếu (người 1 và người 2) theo đúng độ bao phủ của gold dataset.
- `train_15.jpg` & `train_16.jpg`: Bổ sung vị trí mắt cá chân (`left_ankle`, `right_ankle`) bị thiếu và gắn cờ `v = 1` hoặc `v = 2` chính xác theo từng người.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào? Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?**
Lỗi đảo trái/phải xảy ra ở `train_02.jpg`, `train_03.jpg` và `train_17.jpg`. Đây đều là những ảnh có độ phân giải tốt, người đứng rõ ràng (ảnh dễ). Nguyên nhân sai sót là do thao tác nhanh và bị đánh lừa bởi góc nhìn của bức ảnh: khi nhân vật quay mặt đối diện hoặc quay lưng chếch, người gán nhãn theo phản xạ tự nhiên đã lấy bên trái/phải của màn hình máy tính thay vì đặt mình vào hệ quy chiếu giải phẫu của nhân vật trong ảnh.

---

## 3. Kiểm chéo

Bạn cùng nhóm: Nguyễn Văn A (Nhóm 04)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 44% | 18% | 26% | Guideline chưa làm rõ trường hợp tóc dài che tai |
| `right_wrist` | 37% | 22% | 15% | Bạn cùng nhóm tick nhầm `v = 0` thay vì `v = 1` khi cổ tay sau lưng |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Chỉ đánh dấu `v = 2` cho tai khi nhìn thấy rõ từ 50% vành tai trở lên; nếu bị tóc dài, mũ bảo hiểm che khuất lỗ tai ngoài nhưng đầu còn trong ảnh thì bắt buộc tick Occluded (`v = 1`) tại vị trí giải phẫu ước lượng.
- Khớp cổ tay nằm sau lưng hoặc sau tay lái xe vẫn còn trong khung ảnh phải đánh `v = 1` dựa theo hướng cẳng tay, không được tick `v = 0`.

---

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8420 | 0.8510 | +0.0090 |
| pose_mAP50-95 | 0.6120 | 0.6080 | -0.0040 |
| pose_precision | 0.8750 | 0.8810 | +0.0060 |
| pose_recall | 0.8210 | 0.8190 | -0.0020 |
| box_mAP50-95 | 0.7350 | 0.7320 | -0.0030 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - `pose_mAP50-95` giảm nhẹ 0.0040 (-0.4%), trong khi `pose_mAP50` tăng 0.0090 (+0.9%).
   - 20 ảnh với quy tắc gán cờ `v = 1` chặt chẽ của lớp đã dạy model học cách dự đoán vị trí các khớp bị che khuất tốt hơn ở ngưỡng dễ (IoU 0.50). Tuy nhiên, số lượng 20 ảnh là quá nhỏ so với tập dữ liệu gốc của COCO, dẫn đến hiện tượng dịch chuyển phân phối nhẹ (domain shift) và over-fitting trên các tư thế cụ thể của tập train, làm giảm nhẹ độ chính xác ở các ngưỡng IoU khắt khe (0.75 - 0.95).

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` (0.7350) cao hơn `pose_mAP50-95` (0.6120) là 0.1230 (12.3%).
   - Model tìm **người** dễ hơn rất nhiều so với tìm **khớp**. Lý do: Hộp bao (bounding box) tận dụng được toàn bộ đặc trưng ngữ cảnh diện rộng (hình thể, trang phục, đầu tóc), trong khi keypoint là bài toán hồi quy toạ độ chính xác cấp độ điểm ảnh của 17 khớp nhỏ, thường xuyên bị che khuất hoặc xoay chuyển đa dạng.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:**
   - Trong ảnh `test_04.jpg`, người đứng nghiêng bị che một chân: model mắc lỗi **lệch nhẹ** ở khớp cổ chân (`left_ankle`) lệch khoảng 8 pixel so với mắt cá chân thật do viền quần ống rộng gây nhiễu thị giác.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   - Ảnh có OKS thấp nhất là `train_01.jpg` (người thứ 2, OKS 0.881).
   - **Nhãn của tôi đúng hơn**. Trong ảnh này, người thứ hai đứng nép phía sau và tay trái bị che một phần. Tôi đã ước lượng khớp khuỷu tay và cổ tay dựa theo phương giải phẫu của cánh tay, trong khi model bị đánh lừa bởi bóng của người phía trước nên co cụm khớp tay vào sát mép người thứ nhất.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   - Có, ảnh `train_04.jpg` (OKS nhãn vs gold đạt 0.865) cũng là ảnh model cho điểm tự tin và OKS thấp nhất.
   - Điều này chứng minh rằng độ khó của bức ảnh mang tính khách quan: ảnh `train_04.jpg` có góc chụp rộng, độ mờ chuyển động (motion blur) cao, và người chơi bị cắt cụt ở rìa ảnh. Những thách thức thị giác này gây khó khăn tương đồng cho cả người gán nhãn lẫn thuật toán thị giác máy tính.

---

## 5. Một rule evidence bạn đã dùng

- **Đối tượng**: `train_04.jpg`, người thứ 1 (bên trái), khớp `left_ankle` và `right_ankle`.
- **Căn cứ thị giác**: Người vận động viên đang rướn người đánh bóng, hai chân hướng xuống góc dưới bức ảnh. Phần cẳng chân chạm mép đáy ảnh và bị cắt ngang tại vị trí ống đồng, hoàn toàn không có pixel nào của bàn chân hay mắt cá chân xuất hiện trong bức ảnh.
- **Lý do chọn trạng thái**: Khớp đã hoàn toàn rơi ra ngoài khung hình bức ảnh. Theo quy tắc chuẩn của COCO và hướng dẫn lớp, khớp ngoài khung bắt buộc phải tick Outside (`v = 0`) và gán toạ độ `0.0 0.0 0`. Tuyệt đối không được gán toạ độ ảo ngoài khoảng $[0, 1]$ (như $y = 1.38$) với cờ `v = 2` vì sẽ làm sai định dạng YOLO Pose.
