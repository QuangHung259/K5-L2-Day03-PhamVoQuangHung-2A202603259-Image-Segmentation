# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603259
- Ngày / CVAT local: 12/11/2026 – 13/11/2026 / CVAT local (`http://localhost:8080`, v2.74.1)
- Công cụ đã dùng: CVAT local (Polygon, Brush thủ công — không dùng SAM)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | **14 / 14** | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (`640x600`), object đầu tiên khởi tạo bằng Polygon là người đứng trên vỉa hè phía trước cửa hiệu CHANEL ở vùng `(365, 122)–(372, 142)` (16 điểm polygon) và người phụ nữ mặc áo dài trắng đi bộ qua vạch kẻ đường ở trung tâm tiền cảnh tại vùng `(186, 130)–(306, 461)` (75 điểm polygon).
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Dùng công cụ Polygon chấm sát theo đường viền cơ thể và tà áo dài nhìn thấy thực tế (bao gồm phần bàn chân chạm mặt đường, không lấy bóng đổ dưới mặt đường); dừng biên ngay tại ranh giới giao cắt với chiếc xe máy phía sau mà không tự đoán hay vẽ tràn sang phần thân xe máy.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Không dùng.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: **Không dùng** (toàn bộ 62 object trên 3 ảnh của `medium_instance` đều vẽ thủ công bằng Polygon và Brush, `source="manual"`). Khi gán nhãn người điều khiển xe máy ở tiền cảnh trái (`person` tại `(10, 113)–(226, 411)`) và xe máy (`motorcycle` tại `(7, 163)–(119, 310)`), tôi quyết định tách riêng người lái và chiếc xe thành 2 instance độc lập đúng theo class tương ứng, chỉ vẽ phần lộ ra của xe máy mà không gộp người ngồi trên xe vào chung mask `motorcycle`.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng:
  1. Hình học mask: Task `medium_instance`, ảnh `000000373353.jpg`, chiếc xe taxi màu vàng ở giữa đường (`car`, vùng `left=271, top=312, width=100, height=79`) bị người đi bộ (`person`, vùng `(293, 295)–(354, 415)`) bước ngang qua che khuất phần giữa thân xe.
  2. Định dạng export: Task `medium_instance` (`submissions/medium_instance.zip`) khi xuất lần đầu từ CVAT bị chọn nhầm sang định dạng `CVAT for images 1.1` (`annotations.xml`) thay vì `COCO 1.0`.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Lỗi gộp-tách & biên (khi xử lý vật bị che khuất ở `000000373353.jpg`) và lỗi sai định dạng export ZIP.
- Bằng chứng tôi nhìn thấy:
  1. Trên ảnh `000000373353.jpg`, hai chân người đi bộ cắt ngang đầu xe taxi vàng làm phần nhìn thấy của xe bị chia cắt; nếu vẽ một polygon liền mạch sẽ đè lên mask `person`, còn nếu vẽ thành 2 polygon riêng sẽ làm tăng sai số lượng instance `car`.
  2. Khi chạy `python scripts/inspect_submissions.py --dir submissions`, công cụ báo `[LỖI] medium_instance: medium_instance.zip ! cần đúng một COCO JSON trong annotations/`.
- Quy tắc và hành động sửa:
  1. Theo quy tắc vật bị che (occlusion) trong `guideline-mini-sheet.md`, chỉ vẽ phần nhìn thấy và giữ nguyên **một instance** cho cùng một chiếc xe: tôi chuyển sang dùng công cụ Brush (`mask`) để tô hai mảng nhìn thấy của chiếc taxi vàng quanh chân người đi bộ trong cùng 1 object `car` duy nhất (giữ kính chắn gió bên trong mask xe, không đè lên `person`).
  2. Với lỗi định dạng export, kiểm tra lại bằng script tự kiểm để xuất lại từ CVAT theo đúng chuẩn `COCO 1.0` (`annotations/instances_default.json`).
- Sau sửa đã Save và export lại chưa? Đã Save đầy đủ toàn bộ 14/14 ảnh của 9 task trên CVAT local; 8/9 gói ZIP (`easy_semantic.zip`, `hard_panoptic.zip`, `cp1_holes.zip`, `cp2_slice.zip`, `cp3_thin.zip`, `cp4_curb.zip`, `cp5_occlusion.zip`, `cp6_coverage.zip`) đã export đúng chuẩn (`[OK]`), riêng `medium_instance.zip` cần export lại sang `COCO 1.0`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy `python scripts/inspect_submissions.py --dir submissions` kiểm tra cấu trúc: `easy_semantic` `[OK]`, `hard_panoptic` `[OK]` (103 annotations: 95 polygon, 8 RLE), `cp1_holes` `[OK]` (6 polygon), `cp2_slice` `[OK]` (13 polygon), `cp5_occlusion` `[OK]` (48 polygon), `cp3_thin` `[OK]`, `cp4_curb` `[OK]`, `cp6_coverage` `[OK]` / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `medium_instance` — ảnh `000000181542.jpg`, bức ảnh người mẫu lớn trong khung kính cửa hàng CHANEL phía sau (`x≈200–300, y≈40–145`) | **Cách 1:** Gán nhãn `person` cho cả hình người mẫu in trên poster quảng cáo.<br>**Cách 2:** Không gán nhãn `person` cho poster, chỉ gán cho những người thật đứng trên vỉa hè phía trước cửa kính (`(216,119)–(230,147)`, `(285,118)–(308,146)`). | Theo quy tắc instance segmentation, lớp `person` chỉ áp dụng cho thực thể người thật trong cảnh, không áp dụng cho tranh/ảnh phẳng in trên mặt dựng tòa nhà (`building`). | Quyết định: Không vẽ mask cho hình người trên poster CHANEL, chỉ vẽ các instance `person` thật đứng trên vỉa hè phía trước. |
| 2. `hard_panoptic` — ảnh `000000460147.jpg`, xe tải chuyên dụng chở ô tô 2 tầng ở giữa làn đường (`bbox=[314.9, 167.6, 123.35, 203.43]`) | **Cách 1:** Gộp cả xe tải chở xe và các ô tô đang nằm trên sàn rơ-moóc thành một instance `truck` duy nhất.<br>**Cách 2:** Tách riêng phần khung xe tải là `truck` (`id=90`) và từng chiếc ô tô nằm trên sàn chở là các instance `car` riêng biệt (`id=58, 59, 65`). | Trong bài toán Panoptic, mỗi vật đếm được (`thing`) thuộc lớp `car` và `truck` đều có hình dáng và biên nhìn thấy rõ ràng; không gộp nhiều phương tiện khác nhau vào một instance. | Quyết định: Tách riêng từng chiếc `car` trên sàn chở (`id=58, 59, 65`) và gán phần khung/thân rơ-moóc bao quanh là một instance `truck` (`id=90`). |
| 3. `easy_semantic` — ảnh `817bca71-00000000.jpg` và `81ae7cbb-6bc63a4a.jpg`, phần lối xe chạy vào nhà (driveway) cắt ngang qua vỉa hè hai bên đường | **Cách 1:** Tô phần driveway nối từ lòng đường lên sát gara nhà dân là `road` vì xe có thể chạy lên.<br>**Cách 2:** Giữ ranh giới `road` dừng ở mép bó vỉa (curb) của lòng đường chính, phần hành lang đi bộ phía trên bó vỉa gán là `sidewalk`. | Theo `guideline-mini-sheet.md`, ranh giới `road`–`sidewalk` được xác định theo chức năng giao thông chính và đường bó vỉa (curb), không chỉ dựa vào màu sắc hay chất liệu bê tông/nhựa. | Quyết định: Đặt ranh giới `road` bám theo mép bó vỉa dọc lòng đường chính; dải hành lang trên bó vỉa gán là `sidewalk`. |
