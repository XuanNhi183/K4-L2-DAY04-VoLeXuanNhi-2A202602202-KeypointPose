# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Võ Lê Xuân Nhi 

Ngày: 16/9/2026


## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 333 / 133 / 27 |
| Thời gian trung bình mỗi ảnh | 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. right_ear — 55.2%
2. left_ear — 55.2%
3. left_wrist / right_wrist — 37.9% (đồng hạng)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9336 | |
| OKS@0.50 | 1.0 | |
| OKS@0.75 | 1.0 | |
| Lỗi `dao_trai_phai` | 0 | |
| Lỗi `nham_nguoi` | 1 | |
| Lỗi `xoa_khop_bi_che` | 0 | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_04.jpg`, người thứ 1, khớp `left_wrist`: lỗi **nhầm người** — chấm hiện đang gần `left_wrist` của người thứ 2 hơn của người thứ 1 (OKS người này chỉ 0.832, thấp nhất trong 20 ảnh). Sửa: Kéo chấm về đúng cổ tay người thứ 1 rồi chạy lại script.
- `train_13` (right_ear, lệch 12px), `train_15` (right_elbow, lệch 34px), `train_19` (right_ear, lệch 13px), `train_20` (right_wrist, lệch 57px) — 5 lỗi lệch nhẹ này không bắt


**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (`dao_trai_phai` = 0 trong `eval_vs_gold.json`).

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | −0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

`pose_mAP50-95` **tăng** 0.0055 (0.6853 → 0.6908), không giảm. Với chỉ 20 ảnh fine-tune, mức tăng 0.8% này nằm trong khoảng nhiễu bình thường (`pose_mAP50` và `pose_recall` đứng yên tuyệt đối ở 0.845/0.8462) — nên khó khẳng định 20 ảnh "dạy" được điều gì rõ rệt cho model trên tập test này. Điều đáng chú ý hơn là `box_mAP50-95` **giảm** 0.0078 — có thể vì 20 ảnh core nghiêng về những khung cảnh/tư thế khác tập test, khiến model đánh đổi một chút độ chính xác khung bao để khớp tốt hơn các điểm keypoint.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?

Chênh khá lớn: `box_mAP50-95` (0.8119 baseline) cao hơn `pose_mAP50-95` (0.6853 baseline) tới **0.1266**. Model tìm *người* (box) dễ hơn tìm *khớp* (pose) rõ rệt — vì bounding box chỉ cần bao trọn hình dáng người, còn 17 keypoint đòi hỏi định vị chính xác từng điểm nhỏ, dễ sai khi bị che (như tai/cổ tay — đúng nhóm khớp có `%v=1` cao nhất ở Mục 1).

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

`train_13.jpg` có OKS thấp nhất (0.461) trong cả 29 người — thấp đến mức không thể là "lệch nhẹ". Ảnh này có 3 người: một người nhỏ đứng trong tấm gương/phản chiếu bên trái (rất nhỏ, khó định vị), và hai người đứng sát nhau, bounding box chồng nhau (~24%) ở giữa/phải. OKS=0.461 nhiều khả năng rơi vào loại **"trượt hẳn"** (nếu là người nhỏ trong gương — model dễ đặt điểm hoàn toàn sai vị trí vì đối tượng quá nhỏ) hoặc **"nhầm người"** (nếu là một trong hai người chồng nhau — model trộn khớp giữa hai người).

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh có OKS thấp nhất giữa nhãn của tôi và model: **`train_13.jpg`, OKS = 0.461** (thấp nhất trong toàn bộ 29 người, xem bảng bạn vừa gửi). Đây cũng là ảnh có 3 người đứng gần nhau nhất trong tập, phù hợp với việc đông người + chồng lấn làm khó cả việc gán tay lẫn việc model dự đoán.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

**Không trùng nhau.** Ảnh tôi gán tệ nhất so với gold (Mục 2) là `train_04.jpg` (OKS=0.832, lỗi `nham_nguoi` do chính tôi gán nhầm cổ tay sang người khác). Ảnh model lệch nhiều nhất so với nhãn của tôi lại là `train_13.jpg` (0.461) — một ảnh mà theo Mục 2, nhãn của tôi so với gold *không* có vấn đề gì được liệt kê. Điều này cho thấy **hai nguồn lỗi khác nhau**: ở `train_04` là do chính tôi gán sai; ở `train_13` là do bản thân tấm ảnh khó (đông người, một người quá nhỏ trong gương) khiến model dự đoán tệ dù nhãn của tôi đúng. Không nên gộp chung "model sai nhiều" với "nhãn của tôi sai" — đúng như lưu ý ở đầu Mục 4 của template.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

**Ảnh `train_04.jpg`, người thứ 1 (người lái xe máy địa hình bên trái, đội mũ bảo hiểm xám), khớp `right_ankle`** (và tương tự cho `left_knee`, `right_knee`, `left_ankle` của cùng người).
- Trong ảnh, hai điểm hông (`left_hip`, `right_hip`) của người này nằm ở toạ độ y ≈ 0.99 — tức sát ngay mép dưới của khung hình. Từ hông trở xuống (đầu gối, cổ chân) hoàn toàn không xuất hiện trong ảnh: đây không phải là bị xe máy hay vật gì che khuất, mà là do bản thân bức ảnh được chụp cắt cảnh ngang hông (composition dừng ở đó), nên phần chân dưới đơn giản là không tồn tại trong khung. Vì vậy các khớp này được gán `v=0` (không đặt chấm), thay vì `v=1`

-  Theo đúng luật: `v=1` chỉ dùng khi khớp còn **trong khung** nhưng bị vật che, còn `v=0` dùng khi khớp đã **ra ngoài mép ảnh**. Xem `outputs/vis_train/train_04.jpg` để đối chiếu (hông là điểm cam/xanh cuối cùng còn nối được, không có đường nối tiếp xuống chân).


<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
