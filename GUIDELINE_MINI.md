# Mini guideline - nhóm: Nhóm 04  |  người gán: Học viên Lab 4  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm (toạ độ xuất ra file là 0 0 0).
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Căn cứ vào nếp gấp quần, thắt lưng và trục xương đùi nối lên mấu chuyển lớn (greater trochanter) để chấm ước lượng, gắn `v = 1` nếu không lộ rõ đường cong cơ thể. | Hông không bao giờ lộ bề mặt ngoài da khi mặc quần áo rộng; ước lượng theo trục xương đùi giúp model học được độ dài xương đùi chính xác thay vì bị lệch lên eo. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu thấy dái tai hoặc gờ vành tai: đặt chấm tại lỗ tai ngoài và gắn `v = 2`. Nếu bị che kín hoàn toàn nhưng đầu vẫn nằm trọn trong ảnh: ước lượng ngang tầm mắt - sống mũi và gắn `v = 1`. | Tai là mốc quan trọng để suy ra góc quay của đầu (yaw). Giữ `v = 1` giúp model không bỏ quên phần đầu. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ đầu gối/cổ chân rơi ra ngoài rìa ảnh bắt buộc tick Outside (`v = 0`), toạ độ để 0. Tuyệt đối không kéo điểm ra ngoài khung ảnh ($x, y > 1$ hoặc $< 0$). | Toạ độ ngoài ảnh làm vỡ khoảng giá trị $[0, 1]$ của YOLO Pose và gây lỗi định dạng nghiêm trọng khi train/eval. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí khớp cổ tay dựa theo phương của xương cẳng tay (nối từ khuỷu tay xuống), gắn `v = 1`. | Cẳng tay là một đoạn cứng cố định, góc của cẳng tay chỉ thẳng tới vị trí bàn tay/cổ tay kể cả khi bị che khuất. |
| Hai người chồng lên nhau | Hoàn thành dứt điểm từng người một (vẽ box bao quanh người thứ nhất rồi chỉnh đủ 17 điểm, sau đó mới sang người thứ hai). Các khớp bị người kia che tick `v = 1`. | Tránh lỗi nhầm người ("nham_nguoi") - lỗi nguy hiểm thứ hai sau đảo trái/phải, làm model học sai liên kết xương giữa hai cá thể. |
| Người nhỏ đến mức nào thì không gán nữa | Mọi người có chiều cao box $\ge 40$ pixel đều phải gán. Người ở hậu cảnh quá mờ hoặc cao $< 30$ pixel bị cắt nửa thân thì bỏ qua. | Đảm bảo tính nhất quán giữa các thành viên, người quá nhỏ không đủ pixel để phân biệt khớp trái/phải. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1` (bên trái), khớp `left_ankle` / `right_ankle`

- **Mơ hồ ở chỗ nào**: Người chơi thể thao đang vận động mạnh, chân hướng xuống dưới mép đáy bức ảnh. Phần cẳng chân chạm mép ảnh dưới nhưng bàn chân/mắt cá chân không nhìn thấy.
- **Bạn quyết thế nào**: Đánh dấu `v = 0` (Outside) cho cả `left_knee`, `right_knee`, `left_ankle`, `right_ankle` vì toạ độ giải phẫu thực tế rơi vượt quá mép dưới bức ảnh ($y > 1.0$).
- **Vì sao**: Điểm nằm ngoài khung hình thì theo quy tắc COCO bắt buộc phải là `v = 0`, không được đặt chấm có toạ độ vượt ra ngoài khung hình làm sai lệch bounding box và vi phạm khoảng chuẩn hoá $[0, 1]$.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Nếu để `v = 2` hoặc `v = 1` với toạ độ ngoài ảnh, YOLO Pose sẽ nhận toạ độ ngoại lai $> 1.0$, gây lỗi loss NaN hoặc ép model đoán các điểm ảo nằm ngoài ảnh.

### Ca 2 - ảnh `train_01.jpg`, người thứ `2` (bên trái), khớp `left_elbow` / `left_wrist`

- **Mơ hồ ở chỗ nào**: Người đứng chếch nghiêng, cánh tay trái gập ra phía sau lưng và bị thân mình che mất hoàn toàn cẳng tay và cổ tay.
- **Bạn quyết thế nào**: Chấm ước lượng khớp khuỷu tay và cổ tay dựa theo góc nghiêng của vai trái và tư thế đứng, đánh dấu cờ `v = 1` (Occluded).
- **Vì sao**: Toàn bộ cơ thể người này vẫn nằm trọn vẹn bên trong ảnh, khớp chỉ bị che khuất bởi chính cơ thể chứ không ra ngoài mép ảnh. Do đó phải là `v = 1` theo đúng quy ước lớp.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Nếu đánh `v = 0` (Outside), OKS sẽ bỏ qua khớp này, đồng thời làm mất dữ liệu huấn luyện khiến model không học được khả năng suy luận khớp bị che (occlusion reasoning).

### Ca 3 - ảnh `train_02.jpg`, người thứ `1`, khớp `left_hip` / `right_hip`

- **Mơ hồ ở chỗ nào**: Người trong ảnh quay lưng hơi chếch về phía ống kính, mặc trang phục tối màu che kín hông. Rất dễ nhầm lẫn bên trái và bên phải của cơ thể người với bên trái và bên phải của bức ảnh.
- **Bạn quyết thế nào**: Đứng vào vị trí góc nhìn của nhân vật: bên trái cơ thể người hướng về phía bên phải bức ảnh ($x$ lớn hơn). Do đó `left_hip` nằm ở phía bên phải ảnh và `right_hip` nằm ở phía bên trái ảnh. Gắn `v = 1` do bị quần áo che.
- **Vì sao**: Trái/phải luôn tính theo hệ quy chiếu cơ thể người (anatomical left/right), không phụ thuộc vào hướng nhìn của người gán nhãn.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Sẽ gây ra lỗi `dao_trai_phai`. Khi áp dụng data augmentation lật ảnh (`fliplr=0.5`), model sẽ bị dạy sai hai lần và không bao giờ hội tụ đúng về nhận diện tư thế người.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` / `right_ear` (bài của tôi `44%` / bài bạn cùng nhóm `18%`).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline ban đầu chưa làm rõ trường hợp tóc dài che tai: bạn cùng nhóm đánh `v = 2` khi thấy một phần lọn tóc che tai, còn tôi đánh `v = 1` vì cho rằng không thấy rõ lỗ tai và sụn vành tai.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: "Chỉ đánh `v = 2` cho tai khi nhìn thấy rõ ít nhất 50% vành tai hoặc dái tai; nếu tóc hoặc mũ che khuất hoàn toàn lỗ tai ngoài thì bắt buộc tick Occluded (`v = 1`) tại vị trí giải phẫu ước lượng".
