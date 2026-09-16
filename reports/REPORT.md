# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phùng Thảo An   Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 343 / 119 / 31 |
| Thời gian trung bình mỗi ảnh | 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (62%)
2. right_ear (41%)
3. left_wrist (38%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng, đây là những khớp thường xuyên bị che khuất trong tập dữ liệu này. Tuy nhiên, chúng "hay bị che" chứ không hẳn là "khó xác định vị trí giải phẫu". Theo quan sát từ ảnh thực tế, hai tai có tỷ lệ v=1 rất cao do người trong ảnh đội mũ bảo hiểm xe máy. Tương tự, cổ tay trái thường xuyên bị khuất sau tay lái xe máy. Dù tỷ lệ v=1 cao, tôi vẫn có thể dựa vào hướng của đầu mũ bảo hiểm và cẳng tay để ước lượng tự tin tọa độ của các khớp này.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.934 | 0.935 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 2 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- train_03.jpg, người #1, khớp left_hip: Kéo điểm hông trái bị rớt sang cơ thể của người bên cạnh về lại đúng hông của người #1.
- train_04.jpg, người #1, khớp left_wrist: Kéo điểm cổ tay trái bị dính sang cơ thể bên cạnh về lại đúng tay của người #1.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh khi chấm với bộ gold. (Các lỗi đảo trái/phải ở ảnh người đạp xe đã được tôi phát hiện và tự sửa từ bước kiểm tra nội bộ trước đó bằng công cụ trực quan hóa).

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- 

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   > Chỉ số `pose_mAP50-95` **tăng nhẹ (+0.0055)**. Dù chỉ học thêm 20 ảnh, bộ nhãn chất lượng cao đã giúp model tăng độ chính xác (`pose_precision` tăng +0.0058) trong việc bắt tâm khớp mà không đánh rơi khả năng nhận diện tổng thể. Nó không làm hỏng gì đáng kể ở phần pose, dù `box_mAP` có giảm nhẹ đôi chút.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   > `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng **0.1133**. Model tìm *người* (khung box) dễ hơn rất nhiều so với tìm *khớp*. Việc khoanh vùng một khối tổng thể lớn luôn dễ hơn việc phải xác định chính xác tọa độ pixel của 17 điểm nhỏ, đặc biệt khi các điểm này biến dạng liên tục theo tư thế hoặc bị che khuất.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   > Ảnh `test_09.jpg`. Model mắc lỗi **Nhầm người** rất nặng. Do hai người ngồi quá sát nhau trên xe máy, phần khung xương thân dưới và tay của người nam (ngồi trước) và người nữ (ngồi sau) bị kéo dính chéo vào nhau thành một mớ hỗn độn màu xanh/cam.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   > Ảnh `test_07.jpg` (người phụ nữ đứng sau quầy kính). Model đã đoán sai hoàn toàn (Lỗi Trượt hẳn) khi tự động vẽ các điểm đầu gối và mắt cá chân lơ lửng dưới không trung xuyên qua mặt bàn. Con người đúng, vì chúng ta áp dụng luật "mất bằng chứng do bị che/cắt mép thì gán `v=0` (không đặt điểm)", thay vì bắt model phải đoán bừa.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   > Có sự tương đồng. Những tình huống làm khó người gán nhãn nhất (như góc khuất, người chồng chéo lên nhau ở ảnh đi xe máy) cũng chính là những ảnh model vẽ khung xương méo mó nhất (`test_03.jpg`, `test_09.jpg`). Điều này chứng minh rằng khi dữ liệu đầu vào thiếu bằng chứng thị giác, AI cũng "mù mờ" y hệt như con người và không thể tự suy luận ra cấu trúc giải phẫu chuẩn.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

> (1) Ở ảnh `train_04.jpg` (người #1), tôi đã phải quyết định trạng thái cho khớp `left_ear` (tai trái). 
> (2) Bằng chứng thị giác là người này đang đội mũ bảo hiểm trùm kín đầu khi lái xe máy, không thể nhìn thấy lỗ tai bằng mắt thường, nhưng cấu trúc khuôn mặt và quai hàm vẫn hiện rõ. 
> (3) Dựa vào guideline, vì đầu người vẫn nằm hoàn toàn trong khung ảnh và tôi có đủ căn cứ hình học để ước lượng chính xác vị trí tai đằng sau lớp mũ bảo hiểm, tôi đã đặt điểm tọa độ và chọn cờ **`v=1` (Occluded)** thay vì xóa bỏ nó (v=0).

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->