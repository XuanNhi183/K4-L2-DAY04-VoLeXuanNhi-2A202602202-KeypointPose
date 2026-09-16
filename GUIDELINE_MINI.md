# Mini guideline |  người gán: Võ Lê Xuân Nhi  |  ngày: 16/9/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Nếu còn thấy đường viền lưng quần/thắt lưng hoặc nếp gấp eo → `v=2` tại đúng điểm đó. Nếu bị áo/váy dài che kín, không còn mốc nhìn thấy nào → `v=1`, đặt chấm ước lượng theo tỉ lệ vai-hông của người đó. | Số liệu thực tế cho thấy đa số hông trong 20 ảnh đã được gán `v=2` (21/29 trái, 23/29 phải) — tức nhóm đang coi "ước lượng được vị trí giải phẫu" là đủ để tính visible. Cần chốt lại ngưỡng để không mỗi người hiểu một kiểu. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu chỏm tai hoặc dái tai còn lộ ra → `v=2`. Nếu mũ bảo hiểm/nón che kín hoàn toàn cả hai tai → `v=1`, đặt chấm ở vị trí ước lượng theo đường viền mũ. | **Ví dụ thật:** `train_04`, người thứ 1 và người thứ 2 (đội mũ bảo hiểm xe máy) — cả hai đều gán `lear`/`rear` = `v=1`. Đây cũng là khớp có `%v=1` cao nhất toàn bộ tập (55.2%), nên cần luật rõ ràng nhất. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Hông trở lên gán bình thường (`v=2`/`v=1` theo che khuất). Đầu gối, cổ chân **không đặt chấm**, gán `v=0`. | **Ví dụ thật:** `train_04` người thứ 1 và 2, `train_10` người thứ 1, `train_11` người thứ 1 — cả 4 trường hợp đều có `lknee/rknee/lankle/rankle = v=0` vì ảnh cắt ngang người (do khung hình, không phải do vật che). |
| Cổ tay nằm sau tay lái / sau thân mình | Nếu cổ tay còn nằm trong khung ảnh nhưng bị vật (tay lái, thân người, túi...) che → `v=1`, đặt chấm ước lượng theo hướng cẳng tay. Chỉ dùng `v=0` khi cổ tay thực sự vượt ra ngoài mép ảnh. | **Ví dụ thật:** `train_04` người thứ 1 có `rwri = v=0` (tay đưa xuống phía tay lái xe máy, ra khỏi khung) trong khi phần lớn cổ tay bị che khác trong tập đều gán `v=1` (11/29 mỗi bên) — dễ nhầm giữa "che bởi vật" và "ra khỏi khung" nếu không chốt luật này. |
| Hai người chồng lên nhau | Vẽ/kiểm tra **xong hẳn một người** (đủ 17 điểm) rồi mới sang người kia, dựa vào việc bám theo một đường viền cơ thể liên tục thay vì nhảy giữa hai bộ xương. Khớp nào của người bị che bởi người kia vẫn tính theo luật che khuất chung (`v=1` nếu còn ước lượng được). | **Ví dụ thật:** `train_03`, người thứ 1 và người thứ 2 đứng sát nhau, bounding box chồng nhau ~78% — nguy cơ "nhầm người" cao nhất trong tập (xem `outputs/vis_train/train_03.jpg`). |
| Người nhỏ đến mức nào thì không gán nữa | Theo GUIDE.md, bộ ảnh core đã được chọn để mọi người đủ lớn để gán — nhóm **không bỏ qua** ai trong 20 ảnh core. Ngưỡng chỉ áp dụng cho ảnh ngoài core (nếu có): nếu chiều cao bounding box ước lượng < ~3% chiều cao ảnh, không gán. | Không có ca thực tế nào trong 20 ảnh phải bỏ qua vì quá nhỏ, nên đây là luật phòng hờ hơn là luật đã áp dụng — nhóm cần xác nhận lại bằng mắt. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04`, người thứ `1`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Cổ tay phải đưa về phía tay lái xe máy — không rõ là bị **vật che** (nên `v=1`) hay đã **ra khỏi khung hình** (nên `v=0`), vì hai trạng thái trông gần giống nhau khi tay đi khuất sau một vật ở rìa ảnh.
- Bạn quyết thế nào: `v=0` — coi là ra khỏi khung vì điểm ước lượng của cổ tay rơi ra ngoài biên ảnh.
- Vì sao: Mặc dù chỉ khoảng 1/3 cổ tay bị che bởi tay lái, phần lớn cổ tay đã nằm ngoài khung hình.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán `v=1` kèm toạ độ ước lượng trong khung, model sẽ học rằng cổ tay "luôn nằm trong ảnh" ngay cả khi bị cắt cụt, dẫn tới đoán bừa một điểm ảo mỗi khi gặp tay bị cắt mép tương tự.

### Ca 2 - ảnh `train_03`, người thứ `2`, khớp `left_hip` / `right_hip`

- Mơ hồ ở chỗ nào: Người thứ 2 đứng gần sát người thứ 1 (bounding box chồng ~78%), phần hông bị thân người thứ 1 che một phần — khó phân biệt "hông của ai" khi hai đường viền cơ thể gần trùng nhau.
- Bạn quyết thế nào: Bám theo áo khoác da và quần jean của người thứ 2 để xác định đúng hông thuộc về họ, không suy từ vị trí trung bình của cụm hai người.
- Vì sao: Mặc dù người thứ 1 đứng chếch về phía trước và cao hơn một chút, quần áo và tư thế của người thứ 2 cho thấy rõ ràng phần hông của họ nằm ở phía dưới và hơi chếch sang phải so với hông của người thứ 1.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai tương quan hình dạng cơ thể (ví dụ ghép vai người này với hông người kia), khiến pose dự đoán ra một bộ xương "lai" không tồn tại trong thực tế — lỗi `nham_nguoi` kinh điển.

### Ca 3 - ảnh `train_04`, người thứ `1`, khớp `left_ear` / `right_ear`

- Mơ hồ ở chỗ nào: Mũ bảo hiểm che gần hết đầu, chỉ còn lộ một phần khuôn mặt qua kính chắn — không có mốc giải phẫu rõ ràng (như vành tai) để đặt chấm ước lượng.
- Bạn quyết thế nào: Gán `v=1` cho cả hai tai, đặt chấm ở vị trí ước lượng dựa theo đối xứng với mắt và đường viền mũ bảo hiểm hai bên.
- Vì sao: Mặc dù vành tai không lộ, phần lớn mặt và đường viền mũ vẫn hiện rõ, cho phép ước lượng vị trí tai trong khung hình — không đến mức bị che khuất hoàn toàn
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu xoá điểm tai (`v=0`) thay vì ước lượng, model sẽ học rằng "đội mũ bảo hiểm = không có tai" và mất khả năng ước lượng hướng đầu ở các ảnh đội mũ khác — đúng loại lỗi "xoá khớp bị che" mà GUIDE cảnh báo.

<!-- ## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất: -->
