# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603101
- Ngày / CVAT local: 11/11/2026 / CVAT local (http://localhost:8080)
- Công cụ đã dùng: CVAT (Brush, Polygon tool)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, người đi bộ (`person`) ở góc dưới bên phải phía trước camera (tọa độ bbox x: 540.8, y: 147.2, w: 99.2, h: 218.4).
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Quy tắc biên: bám sát theo phần cơ thể và trang phục nhìn thấy thực tế (viền đầu, mũ len, áo khoác, quần, viền đế giày), dừng biên đúng mép ngoài tiếp xúc của đế giày và không vẽ lấn vào bóng đổ của người trên mặt đường.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: không dùng gợi ý tự động cho object này; tự vẽ thủ công hoàn toàn bằng Polygon để kiểm soát chặt chẽ từng điểm biên mép người và loại bỏ triệt để phần bóng đổ mặt đường.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: không dùng; quyết định đi các điểm polygon bám sát đường viền quần áo và đế giày của người đi bộ, không mở rộng mask sang bóng đổ dưới chân và không ăn sang mặt đường.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, ảnh `000000017627.jpg`, cụm xe ô tô đỗ sát cạnh nhau ở làn đường bên trái (khu vực x: 29-121, y: 215-325).
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: gộp-tách (gộp nhầm 2 xe đỗ sát nhau thành 1 mask).
- Bằng chứng tôi nhìn thấy: Ban đầu vẽ một polygon bao trùm toàn bộ hai xe cạnh nhau vì xe trước và xe sau đỗ áp sát nhau tạo cảm giác như một khối liên tục; khi zoom lớn vào khu vực tiếp giáp nhận thấy có viền nóc xe và khe hở kính chắn gió phân tách rõ ràng hai thân xe.
- Quy tắc và hành động sửa: Quy tắc instance segmentation yêu cầu mỗi xe là một instance riêng biệt ("cùng class không đồng nghĩa cùng instance"). Đã xóa polygon gộp chung và vẽ lại thành 2 polygon riêng biệt cho từng xe (`car #1` và `car #2`).
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT và export lại file `cp2_slice.zip` theo định dạng COCO 1.0.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy script `python scripts/inspect_submissions.py`, task `cp2_slice` đạt trạng thái `[OK]` với 13 annotations đa giác riêng biệt hợp lệ; chưa có điểm chính thức do chờ reference phát hành. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 | Ảnh `000000144300.jpg` (`cp1_holes`), kính lái và cửa sổ cabin xe tải (`truck`) | Khoét lỗ cửa kính coi như background hay giữ nguyên kính trong mask của xe tải | Quy tắc checkpoint cp1_holes: kính xe, khe hở trên thân xe là chi tiết cấu thành bên trong vật, không phải lỗ nền thật; không tự ý cắt thủng mask. | Quyết định giữ kín toàn bộ phần kính xe và cabin bên trong mask của xe tải `truck`, không khoét lỗ. |
| 2 | Ảnh `7d83710e-4697c3b2.png` (`cp4_curb`), đoạn đường tiếp giáp vỉa hè có màu nhựa đường và bê tông gần giống nhau | Gộp toàn bộ thành `road` theo cảm nhận màu sắc hay tách riêng `road` và `sidewalk` | Quy tắc checkpoint cp4_curb: phân định ranh giới theo chức năng di chuyển và gờ bó vỉa (curb), không quyết định class chỉ theo màu sắc quang học. | Phóng to xác định rõ gờ mép bó vỉa để vẽ ranh giới tách biệt giữa lòng đường (`road`) và vỉa hè (`sidewalk`). |
| 3 | Ảnh `000000336232.jpg` (`cp5_occlusion`), xe ô tô bị cột đèn/biển báo che cắt ngang thân xe thành hai mảng rời rạc | Tách thành 2 object riêng hay gộp chung 1 instance ID | Quy tắc checkpoint cp5_occlusion: vật thể bị che khuất ngắt quãng vẫn là một instance duy nhất; không vẽ phần bị che nhưng không tách ID. | Gán các phần nhìn thấy của xe thuộc cùng 1 mask instance (`car`), không tách thành 2 ID khác nhau và không vẽ đè lên cột che. |
